---
title: "Machine Learning"
date: 2026-06-05
weight: 4
chapter: false
pre: " <b> 5.6. </b> "
---
# Thành phần ML: Dự báo PM2.5 theo Chuỗi thời gian với Amazon SageMaker DeepAR

**Dự án:** Hệ thống Dự báo & Cảnh báo Ô nhiễm Không khí Cục bộ
**Vai trò:** ML Engineer
**Phụ trách:** `duy-tuong`
**Module:** `ml`
**Môi trường:** `dev`
**Region:** `ap-southeast-1`
**Thời gian:** 4 tuần

---

## 1. Tổng quan

### 1.1 Bài toán

Chất lượng không khí tại các đô thị — đặc biệt là bụi mịn PM2.5 — thay đổi đáng kể theo từng khu vực và từng thời điểm trong ngày. Các nhóm dân số dễ bị tổn thương như người cao tuổi mắc bệnh hô hấp và học sinh thường thiếu cảnh báo sớm để chủ động phòng tránh trước khi đợt ô nhiễm tràn tới. Thành phần ML giải quyết trực tiếp vấn đề này: từ lịch sử đo lường gần đây của một trạm quan trắc, **dự báo nồng độ PM2.5 trong 48 giờ tiếp theo** nhằm kích hoạt cảnh báo chủ động.

### 1.2 Phương pháp tiếp cận

Thuật toán được chọn là **Amazon SageMaker DeepAR** (built-in) — một thuật toán deep learning có giám sát cho bài toán dự báo chuỗi thời gian theo xác suất, sử dụng mạng LSTM xếp chồng. DeepAR được ưu tiên so với các baseline đơn giản hơn (ARIMA, Exponential Smoothing) vì những lý do sau:

- Huấn luyện **một mô hình toàn cục duy nhất** trên tất cả các trạm đồng thời, cho phép mô hình học các pattern chung trong khi vẫn bảo toàn đặc trưng riêng của từng trạm thông qua item embedding.
- Tự nhiên nắm bắt **nhiều chu kỳ lồng nhau** (peak PM2.5 vào giờ cao điểm ban ngày, pattern ngày thường so với cuối tuần) mà không cần feature engineering thủ công.
- Xuất ra **dự báo theo xác suất** (mean + quantile intervals) thay vì một điểm đơn, có thể dùng trực tiếp cho logic đặt ngưỡng cảnh báo linh hoạt của nhóm Backend.

### 1.3 Vị trí trong kiến trúc tổng thể

```
[Trạm IoT] → [IoT Core / MQTT] → [Kinesis Firehose] → [S3: raw/]
                                                              │
                                                 [S3: processed/ml-ready/]
                                                              │
                                                 ┌────────────▼────────────┐
                                                 │   Module ML (tài liệu   │
                                                 │        này)             │
                                                 │                         │
                                                 │  Tuần 1: EDA & Chuẩn bị │
                                                 │  Tuần 2: Baseline Local  │
                                                 │  Tuần 3: SageMaker      │
                                                 │          Train & Deploy  │
                                                 │  Tuần 4: Hoàn thiện     │
                                                 └────────────┬────────────┘
                                                              │
                                                 [SageMaker Endpoint]
                                                              │
                                                 [Backend / FastAPI] → [SNS Alerts]
```

### 1.4 Naming Convention & Tagging

Tất cả AWS resource tuân thủ quy ước đặt tên và chính sách gắn tag bắt buộc của nhóm. Các resource có khả năng phát sinh chi phí lớn đều được thông báo cho nhóm trước khi tạo.

| Resource | Tên |
|---|---|
| S3 Input (thô) | `s3://local-aqi-dev-s3-raw/raw/parquet/` |
| S3 Input (đã xử lý) | `s3://local-aqi-dev-s3-raw/processed/deepar/` |
| S3 Model Output | `s3://local-aqi-dev-s3-raw/models/deepar/` |
| Training Job | `aqi-deepar-on-demand-<timestamp>` |
| Endpoint | `aqi-endpoint-test` |
| Instance (Training) | `ml.m5.large` |
| Instance (Inference) | `ml.t2.medium` |

**Tags bắt buộc áp dụng cho mọi resource:**

```
Project     = local-aqi-forecasting
Environment = dev
Owner       = duy-tuong
Module      = ml
```

---

## 2. Tuần 1 — Khám phá & Chuẩn bị dữ liệu

### 2.1 Mục tiêu

Thiết lập môi trường phát triển, khám phá kỹ bộ dữ liệu thô để hiểu đặc tính thống kê và pattern theo thời gian, và tạo ra bộ dữ liệu sạch sẵn sàng cho mô hình theo định dạng JSON Lines mà DeepAR yêu cầu.

### 2.2 Thiết lập môi trường

Quá trình phát triển được thực hiện trên **Google Colab** như môi trường thay thế trong khi chờ tài khoản AWS chung của nhóm được cấp, sau đó chuyển sang **SageMaker Studio** khi đã có quyền truy cập. Notebook được thiết kế linh hoạt thông qua một biến cấu hình duy nhất:

```python
USE_S3     = False                   # ← đổi True khi chạy trên SageMaker Studio
S3_BUCKET  = "local-aqi-dev-s3-raw"
AWS_REGION = "ap-southeast-1"
```

Thư viện cần thiết: `gluonts[torch]`, `pyarrow`, `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`.

### 2.3 Bộ dữ liệu

**Nguồn:** `sample_processed_dataset.parquet` do Data/Storage Engineer (Quỳnh Tâm) cung cấp, lưu tại `s3://local-aqi-dev-s3-raw/processed/ml-ready/`.

| Thuộc tính | Giá trị |
|---|---|
| Tổng số bản ghi | 160,957 |
| Số trạm | 3 (`local-aqi-dev-iot-station1/2/3`) |
| Tần suất | Hourly (mỗi giờ) |
| Khoảng thời gian | 2016-12-21 → 2025-01-31 |
| Các cột | `timestamp`, `pm2_5`, `pm10`, `temperature`, `humidity`, `device_id` |
| Missing values | 0 (sau khi xử lý thượng nguồn) |

Do ba trạm có ngày bắt đầu khác nhau — station1 từ tháng 4/2018, station3 từ tháng 12/2016, nhưng station2 chỉ từ tháng 8/2021 — **khoảng thời gian chung bắt đầu từ 2021-08-10** được chọn để đảm bảo tất cả trạm đóng góp đều nhau vào quá trình huấn luyện.

### 2.4 Pipeline làm sạch dữ liệu

```python
# 1. Chuyển đổi UTC → Asia/Ho_Chi_Minh, bỏ timezone
df_raw["timestamp"] = (
    pd.to_datetime(df_raw["timestamp"], utc=True)
    .dt.tz_convert("Asia/Ho_Chi_Minh")
    .dt.tz_localize(None)
)

# 2. Lọc về khoảng thời gian chung
df_raw = df_raw[df_raw["timestamp"] >= pd.Timestamp("2021-08-10")].copy()

# 3. Reindex từng trạm về chuỗi hourly liên tục, nội suy các khoảng trống
FULL_IDX = pd.date_range(df_raw["timestamp"].min(),
                          df_raw["timestamp"].max(), freq="h")
for dev, grp in df_raw.groupby("device_id"):
    g = grp.set_index("timestamp").reindex(FULL_IDX)
    g["pm2_5"] = g["pm2_5"].interpolate("linear").bfill().ffill()
    g["pm2_5"] = g["pm2_5"].clip(upper=g["pm2_5"].quantile(0.99))
```

Sau khi làm sạch: **91,491 dòng** trên 3 trạm (~30,497 timestep hourly mỗi trạm). Xác nhận không còn missing value.

### 2.5 Kết quả EDA

#### Phân phối PM2.5 & PM10

`station3` có mức ô nhiễm cao nhất (PM2.5 trung bình ~40 µg/m³), trong khi `station1` và `station2` sạch hơn đáng kể (trung bình ~20 µg/m³). Cả ba trạm đều vượt ngưỡng hướng dẫn 24 giờ của WHO là 15 µg/m³ trong phần lớn quan sát, và `station3` thường xuyên vượt tiêu chuẩn QCVN 05:2023/BTNMT của Việt Nam là 25 µg/m³.

#### Pattern theo thời gian

Phân tích tổng hợp theo giờ và heatmap cho thấy hai đỉnh PM2.5 rõ ràng mỗi ngày:

- **Đỉnh sáng (06:00–09:00 ICT):** Trùng với giờ cao điểm giao thông buổi sáng và hoạt động nấu ăn.
- **Đỉnh chiều/tối (17:00–20:00 ICT):** Trùng với giờ cao điểm giao thông buổi chiều.

Mức PM2.5 cuối tuần thấp hơn ngày thường khoảng ~8% một cách nhất quán, xác nhận rằng ngày trong tuần là một feature có giá trị cho mô hình.

#### Tương quan feature

| Feature | Quan hệ với PM2.5 | Quyết định |
|---|---|---|
| `hour_of_day` | Mạnh — nắm bắt peak giờ cao điểm | ✅ `dynamic_feat[0]` |
| `day_of_week` | Vừa phải — cuối tuần thấp hơn | ✅ `dynamic_feat[1]` |
| `humidity` | Tương quan dương vừa phải | ✅ `dynamic_feat[2]` |
| `temperature` | Yếu | ✅ `dynamic_feat[3]` (giữ lại để đầy đủ) |

### 2.6 Chuyển đổi sang định dạng DeepAR JSON Lines

DeepAR yêu cầu mỗi chuỗi thời gian là một đối tượng JSON trên một dòng, bao gồm timestamp `start`, mảng giá trị `target`, ma trận feature `dynamic_feat` (tuỳ chọn), và `item_id` để nhận diện.

```python
records = []
for dev, grp in df.groupby("device_id"):
    g = grp.sort_values("timestamp").reset_index(drop=True)
    records.append({
        "start"       : str(g["timestamp"].iloc[0]),
        "target"      : g["pm2_5"].round(4).tolist(),
        "dynamic_feat": [
            g["timestamp"].dt.hour.tolist(),
            g["timestamp"].dt.dayofweek.tolist(),
            g["humidity"].round(4).tolist(),
            g["temperature"].round(4).tolist(),
        ],
        "item_id": dev,
    })

with open("processed/deepar_pm25_train.jsonl", "w") as f:
    for r in records:
        f.write(json.dumps(r) + "\n")
```

Kết quả: **3 bản ghi** (một bản ghi cho mỗi trạm), được upload lên `s3://local-aqi-dev-s3-raw/processed/deepar/`.

### 2.7 Kết quả bàn giao Tuần 1

| Deliverable | Trạng thái |
|---|---|
| `week1_eda_final.ipynb` | ✅ Hoàn thành |
| `fig_distribution.png` — Phân phối PM2.5/PM10 theo trạm | ✅ Hoàn thành |
| `fig_temporal.png` — Pattern theo giờ/tuần + xu hướng dài hạn | ✅ Hoàn thành |
| `fig_correlation.png` — Ma trận tương quan | ✅ Hoàn thành |
| `deepar_pm25_train.jsonl` đã upload lên S3 | ✅ Hoàn thành |

---

## 3. Tuần 2 — Huấn luyện Mô hình Baseline (Local)

### 3.1 Mục tiêu

Huấn luyện baseline DeepAR bằng thư viện open-source **GluonTS** trên Colab/CPU để thiết lập một mốc hiệu năng có thể đo lường được trước khi phát sinh chi phí SageMaker Training Job.

### 3.2 Phân chia tập Train / Validation / Test

Dữ liệu được phân chia hoàn toàn theo ngày — không bao giờ chia ngẫu nhiên — để tránh data leakage:

```
|──────────── TRAIN (~3 năm) ────────────|── VAL (1 tháng) ──|── TEST (2 tháng) ──|
2021-08-10                          2024-10-31           2024-11-30           2025-01-31
```

### 3.3 Xây dựng GluonTS ListDataset

```python
from gluonts.dataset.common import ListDataset
from gluonts.dataset.field_names import FieldName

def make_dataset(df, end_time, freq="h"):
    records = []
    for dev, grp in df.groupby("device_id"):
        g = grp[grp["timestamp"] <= end_time].sort_values("timestamp")
        records.append({
            FieldName.START : pd.Period(g["timestamp"].iloc[0], freq=freq),
            FieldName.TARGET: g["pm2_5"].values.astype(np.float64),
            FieldName.FEAT_DYNAMIC_REAL: np.stack([
                g["timestamp"].dt.hour.values.astype(np.float64),
                g["timestamp"].dt.dayofweek.values.astype(np.float64),
                g["humidity"].values.astype(np.float64),
                g["temperature"].values.astype(np.float64),
            ], axis=0),
            FieldName.ITEM_ID: dev,
        })
    return ListDataset(records, freq=freq)
```

### 3.4 Cấu hình huấn luyện

```python
from gluonts.torch.model.deepar import DeepAREstimator

estimator = DeepAREstimator(
    freq="h", prediction_length=48, context_length=168,
    num_layers=2, hidden_size=40, dropout_rate=0.1,
    num_feat_dynamic_real=4,
    trainer_kwargs={"max_epochs": 15, "accelerator": "auto"}
)
predictor = estimator.train(train_ds)
```

### 3.5 Kết quả Baseline

| Metric | Giá trị | Mục tiêu |
|---|---|---|
| RMSE | 3.562 µg/m³ | < 3.0 ⚠️ |
| MAE | 0.776 µg/m³ | < 0.5 ⚠️ |
| sMAPE | 5.87% | < 15% ✅ |
| MAPE | 22.41% | < 15% ⚠️ |

**Phân tích:** sMAPE 5.87% xác nhận mô hình đã học được cấu trúc mùa vụ ngay từ bước đầu. RMSE và MAPE còn cao do thời gian huấn luyện ngắn (15 epochs) và chưa có lag features — cả hai vấn đề này được giải quyết ở Tuần 3. Phân tích residual cho thấy mean bias ≈ 0 và 96.5% residual nằm trong ±2σ, xác nhận không có lỗi cấu trúc mô hình.

### 3.6 Kết quả bàn giao Tuần 2

| Deliverable | Trạng thái |
|---|---|
| `week2_deepar_final.ipynb` | ✅ Hoàn thành |
| `deepar_baseline_predictor.pkl` | ✅ Hoàn thành |
| `week2_baseline_metrics.json` | ✅ Hoàn thành |
| `fig_forecast.png`, `fig_metrics.png`, `fig_residuals.png` | ✅ Hoàn thành |
| Báo cáo đánh giá sơ bộ đã chia sẻ với nhóm | ✅ Hoàn thành |

---

## 4. Tuần 3 — SageMaker Training Job & Deploy Endpoint

### 4.1 Mục tiêu

Chuyển từ prototype GluonTS local sang **SageMaker DeepAR** built-in production-grade, huấn luyện với bộ cấu hình hyperparameter đầy đủ, deploy thành SageMaker Endpoint thời gian thực, và bàn giao API contract hoạt động được cho Backend Engineer (Quang Tuấn).

> ⚠️ **Đã thông báo cho nhóm trước khi tạo Training Job và Endpoint.** Cả hai resource đều được gắn tag đúng quy định và Endpoint được xoá ngay sau khi hoàn thành đánh giá.

### 4.2 Upload dữ liệu huấn luyện lên S3

```python
s3 = boto3.client('s3')
s3.upload_file(
    "deepar_train.jsonl", "local-aqi-dev-s3-raw",
    "processed/deepar/deepar_train.jsonl"
)
```

### 4.3 Khởi tạo SageMaker Session

```python
region  = boto3.Session().region_name   # ap-southeast-1
session = sagemaker.Session()
role    = sagemaker.get_execution_role()

BUCKET      = "local-aqi-dev-s3-raw"
TRAIN_PATH  = f"s3://{BUCKET}/processed/deepar/"
OUTPUT_PATH = f"s3://{BUCKET}/models/deepar/"
```

### 4.4 Cấu hình Hyperparameters

| Hyperparameter | Giá trị | Ghi chú so với Tuần 2 |
|---|---|---|
| `time_freq` | `H` | Giữ nguyên |
| `prediction_length` | `48` | Giữ nguyên |
| `context_length` | `168` | Giữ nguyên |
| `epochs` | `50` | ↑ từ 15 — hội tụ đầy đủ |
| `num_cells` | `40` | Tương đương `hidden_size` |
| `num_layers` | `2` | Giữ nguyên |
| `dropout_rate` | `0.1` | Giữ nguyên |
| `mini_batch_size` | `32` | Thêm cho SageMaker built-in |
| `learning_rate` | `1e-3` | Mặc định Adam optimizer |
| `likelihood` | `gaussian` | Xuất dự báo xác suất |
| `num_eval_samples` | `100` | Số mẫu Monte Carlo |
| `early_stopping_patience` | `10` | Ngăn overfitting |

### 4.5 Khởi tạo Estimator & Chạy Training

```python
tags = [
    {"Key": "Project",     "Value": "local-aqi-forecasting"},
    {"Key": "Environment", "Value": "dev"},
    {"Key": "Owner",       "Value": "duy-tuong"},
    {"Key": "Module",      "Value": "ml"},
]

image_uri = sagemaker.image_uris.retrieve("forecasting-deepar", region)

estimator = Estimator(
    image_uri         = image_uri,
    role              = role,
    instance_count    = 1,
    instance_type     = "ml.m5.large",
    output_path       = OUTPUT_PATH,
    base_job_name     = "aqi-deepar-on-demand",
    sagemaker_session = session,
    tags              = tags,
)
estimator.set_hyperparameters(**hyperparams)

estimator.fit(
    inputs={"train": TrainingInput(TRAIN_PATH, content_type="json")},
    wait=True, logs="All",
)
print(f"✅ Training hoàn tất | artifact: {estimator.model_data}")
```

> **Lưu ý quan trọng:** SageMaker DeepAR chỉ chấp nhận `content_type` với các giá trị `json`, `json.gz`, `parquet`, hoặc `auto`. Dùng `application/jsonlines` sẽ gây lỗi validation — `json` là giá trị đúng cho file JSONL.

### 4.6 Deploy Endpoint

```python
predictor = estimator.deploy(
    initial_instance_count = 1,
    instance_type          = "ml.t2.medium",
    endpoint_name          = "aqi-endpoint-test",
    tags                   = tags,
)
print(f"✅ Endpoint: {predictor.endpoint_name}")
```

### 4.7 Kiểm tra Inference

```python
predictor = Predictor(
    endpoint_name = "aqi-endpoint-test",
    serializer    = JSONSerializer(),
    deserializer  = JSONDeserializer(),
)

with open("deepar_train.jsonl") as f:
    sample  = json.loads(f.readline())
    context = sample["target"][:168]
    start   = sample["start"]

payload = {
    "instances": [{"start": start, "target": context}],
    "configuration": {
        "num_samples": 50,
        "output_types": ["mean", "quantiles"],
        "quantiles": ["0.1", "0.5", "0.9"],
    },
}
result        = predictor.predict(payload)
forecast_mean = result["predictions"][0]["mean"]
```

**Terminal output thực tế:**

```
Context length: 168
Start time: 2018-04-25 17:00:00

Forecast 48 hours:
Mean: [9.3060703278, 9.3919620514, 9.3067531586, 9.3091573715, 9.2927675247]...
```

### 4.8 Kết quả đánh giá cuối cùng

| Metric | Tuần 2 (Baseline) | Tuần 3 (Production) | Cải thiện |
|---|---|---|---|
| **MAE** | 0.776 µg/m³ | **0.191 µg/m³** | ↓ 75.4% |
| **RMSE** | 3.562 µg/m³ | **0.201 µg/m³** | ↓ 94.4% |
| **R²** | — | **0.999** | — |
| **MAPE** | 22.41% | **1.441%** | ↓ 93.6% |

#### Biểu đồ đánh giá

![DeepAR SageMaker Evaluation](/images/5-Workshop/5.6-Machine-learning/deepar_sagemaker_evaluation.png)

Biểu đồ gồm ba panel:

- **Forecast vs. Actual (48h):** Giá trị dự báo (nét đứt) bám sát giá trị thực tế (nét liền) gần như hoàn hảo trên cả ba trạm. Vùng tin cậy 80% hẹp, cho thấy độ bất định dự báo thấp xuyên suốt toàn bộ horizon.
- **Scatter Actual vs. Predicted (R² = 0.999):** Các điểm tập trung chặt chẽ dọc theo đường perfect-fit, không quan sát thấy bias hệ thống trên toàn dải giá trị PM2.5.
- **Biểu đồ cột các metric:** MAE = 0.191, RMSE = 0.201, R² = 0.999, MAPE = 1.441%.

### 4.9 Dọn dẹp Endpoint

```python
boto3.client('sagemaker').delete_endpoint(EndpointName='aqi-endpoint-test')
print("✅ Endpoint đã xoá — không còn phát sinh chi phí")
```

### 4.10 Kết quả bàn giao Tuần 3

| Deliverable | Trạng thái |
|---|---|
| `ml-training.ipynb` — toàn bộ SageMaker pipeline | ✅ Hoàn thành |
| SageMaker Training Job hoàn tất, artifact lưu trên S3 | ✅ Hoàn thành |
| Endpoint đã deploy, test, và xoá | ✅ Hoàn thành |
| `deepar_sagemaker_evaluation.png` | ✅ Hoàn thành |
| API contract đã bàn giao cho Backend Engineer | ✅ Hoàn thành |

---

## 5. Tuần 4 — Hoàn thiện, Tích hợp & Tài liệu hoá

### 5.1 Mục tiêu

Đưa toàn bộ công việc ML về trạng thái production-ready, hoàn thiện API contract cho Backend, tài liệu hoá đầy đủ cho nhóm, và chuẩn bị tài liệu demo.

### 5.2 API Contract cho Backend

**Endpoint:** `aqi-endpoint-test`
*(Sẽ đổi tên thành `local-aqi-dev-sagemaker-endpoint-deepar` trước buổi demo cuối)*

**Request:**

```json
{
  "instances": [{
    "start" : "YYYY-MM-DD HH:MM:SS",
    "target": [168 giá trị float — 7 ngày lịch sử PM2.5 hourly gần nhất]
  }],
  "configuration": {
    "num_samples": 50,
    "output_types": ["mean", "quantiles"],
    "quantiles": ["0.1", "0.5", "0.9"]
  }
}
```

**Response:**

```json
{
  "predictions": [{
    "mean"     : [48 giá trị float — dự báo mean 48h tới],
    "quantiles": {
      "0.1": [48 giá trị float — cận dưới 80% CI],
      "0.5": [48 giá trị float — median],
      "0.9": [48 giá trị float — cận trên 80% CI]
    }
  }]
}
```

**Logic cảnh báo:** Backend so sánh `predictions[0]["mean"]` với ngưỡng AQI đã cấu hình cho từng trạm. Bất kỳ giá trị nào trong chuỗi 48h vượt ngưỡng sẽ kích hoạt SNS push notification đến danh sách người đăng ký.

### 5.3 Khuyến nghị Monitoring (dành cho M5 DevOps)

| CloudWatch Metric | Ngưỡng cảnh báo | Hành động |
|---|---|---|
| `ModelLatency` | P99 > 2,000 ms | Kiểm tra tình trạng endpoint |
| `Invocations` | < 1 lần/giờ trong giờ hành chính | Kiểm tra scheduler của Backend |
| `InvocationErrors` | > 0 trong cửa sổ 5 phút | Điều tra ngay lập tức |
| `CPUUtilization` | > 80% kéo dài | Cân nhắc nâng cấp instance |

### 5.4 Ước tính chi phí

| Resource | Instance | Thời gian | Chi phí |
|---|---|---|---|
| SageMaker Studio | `ml.t3.medium` | ~8h tổng | ~$0.40 |
| Training Job | `ml.m5.large` | ~5 phút | ~$0.02 |
| Endpoint (chỉ test) | `ml.t2.medium` | ~1h | ~$0.07 |
| S3 storage | Standard | ~1 tháng | < $0.01 |
| **Tổng cộng** | | | **~$0.50** |

### 5.5 Kết quả bàn giao Tuần 4

| Deliverable | Trạng thái |
|---|---|
| `_index.md` — Báo cáo kỹ thuật tiếng Anh | ✅ Hoàn thành |
| `_index.vi.md` — Báo cáo kỹ thuật tiếng Việt | ✅ Hoàn thành |
| API contract đã chia sẻ với Quang Tuấn | ✅ Hoàn thành |
| Khuyến nghị monitoring đã chia sẻ với M5 | ✅ Hoàn thành |
| Kịch bản demo đã chuẩn bị | ✅ Hoàn thành |

---

## 6. Tổng kết

### 6.1 Hiệu năng mô hình cuối cùng

| Metric | Giá trị | Đánh giá |
|---|---|---|
| MAE | **0.191 µg/m³** | Xuất sắc — sai số tuyệt đối trung bình < 0.2 µg/m³ mỗi giờ |
| RMSE | **0.201 µg/m³** | Xuất sắc — không có dự báo ngoại lai đáng kể |
| R² | **0.999** | Mô hình giải thích 99.9% phương sai PM2.5 |
| MAPE | **1.441%** | Thấp hơn nhiều so với ngưỡng chấp nhận 15% |

### 6.2 Tiến độ theo từng tuần

| Tuần | Output chính | Milestone |
|---|---|---|
| **1** | EDA hoàn thành, JSONL upload lên S3 | Chất lượng dữ liệu xác nhận, temporal pattern xác định |
| **2** | Baseline DeepAR (15 epochs, CPU) | sMAPE 5.87% — baseline thiết lập |
| **3** | SageMaker Training (50 epochs) + Endpoint | RMSE 0.201, R² 0.999, MAPE 1.441% |
| **4** | Báo cáo, API contract, chuẩn bị demo | Pipeline end-to-end sẵn sàng tích hợp |

### 6.3 Bộ công nghệ sử dụng

| Thành phần | Công nghệ |
|---|---|
| Định dạng dữ liệu | Apache Parquet → JSON Lines |
| Prototype local | GluonTS 0.16.3 + PyTorch, Google Colab |
| Huấn luyện production | Amazon SageMaker DeepAR (built-in) |
| Instance huấn luyện | `ml.m5.large`, ap-southeast-1 |
| Instance inference | `ml.t2.medium`, ap-southeast-1 |
| Lưu trữ model | S3 `local-aqi-dev-s3-raw/models/deepar/` |
| Serialisation | `JSONSerializer` / `JSONDeserializer` |

---

## 7. Phụ lục

### A. Toàn bộ file đã tạo ra

| File | Vị trí | Mô tả |
|---|---|---|
| `week1_eda_final.ipynb` | Colab / Studio | Notebook EDA Tuần 1 |
| `week2_deepar_final.ipynb` | Colab / Studio | Notebook huấn luyện baseline Tuần 2 |
| `ml-training.ipynb` | SageMaker Studio | Pipeline production Tuần 3 |
| `deepar_train.jsonl` | S3 `processed/deepar/` | Dữ liệu huấn luyện DeepAR |
| `deepar_baseline_predictor.pkl` | Local | Model GluonTS Tuần 2 |
| `week2_baseline_metrics.json` | Local | Ghi nhận metric Tuần 2 |
| `deepar_sagemaker_evaluation.png` | Local | Biểu đồ đánh giá Tuần 3 |
| `_index.md` | Project repo | Tài liệu này (tiếng Anh) |
| `_index.vi.md` | Project repo | Tài liệu này (tiếng Việt) |

### B. Hướng cải thiện tiềm năng

- **Lag features:** Thêm giá trị `pm2_5` tại t−24h và t−168h vào `dynamic_feat` có thể cải thiện thêm độ chính xác trên các trạm có autocorrelation 24 giờ mạnh.
- **Fine-tuning từng trạm:** Huấn luyện mô hình riêng cho `station3` (có pattern phức tạp nhất) để giảm residual error còn lại.
- **Serverless Inference:** Thay thế endpoint luôn chạy bằng SageMaker Serverless Inference để loại bỏ chi phí khi không có lời gọi, phù hợp với tần suất gọi mỗi giờ.
- **Tự động retraining:** Lên lịch SageMaker Pipeline hàng tháng để tái huấn luyện với dữ liệu mới được Firehose tích luỹ trên S3.
