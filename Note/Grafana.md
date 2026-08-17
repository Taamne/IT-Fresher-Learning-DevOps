# Grafana

Grafana không phải nơi tạo ra dữ liệu

Grafana là công cụ trực quan hóa dữ liệu

Trong Grafana, **Data Source** là nguồn mà Grafana lấy dữ liệu.

Data Source = "Grafana hỏi dữ liệu ở đâu?”

Grafana sẽ gửi query sang Data Source

Metric là **số liệu đo lường theo thời gian**.

Một metric thường có dạng:

```
metric_name{label1="value", label2="value"}
```

Label

**label là thông tin đi kèm metric để mô tả metric đó thuộc về ai, cái gì, ở đâu hoặc trạng thái nào**.

Ví dụ có metric:

```
system_cpu_time_seconds_total
```

⇒ chỉ nhìn tên metrix này chỉ biết đây là dữ liệu về CPU

Ví dụ đầy đủ có label:

```
system_cpu_time_seconds_total{
  instance="10.10.20.15:8889",
  host_name="linux-server-01",
  cpu="0",
  state="idle"
}
```

Đây là metric CPU của máy `linux-server-01`, được lấy qua instance `10.10.20.15:8889`, của CPU core `0`, và đang đo thời gian ở trạng thái `idle`.

Query

Grafana phải query đến data source (dùng ngôn ngữ để query)

Metric
↓
 query
↓
Grafana nhận kết quả
↓
Panel hiển thị

Dashboard → Panel

**Dashboard** là một màn hình tổng hợp.

**Panel** là từng ô biểu đồ.

Phân biệt Metric, Log và Trace

```
         Observability

  Metric      Log       Trace
    │          │          │
    │          │          │
 Grafana     Loki       Tempo
    │
```

Prometheus

Ví dụ server bị chậm.

**Metric** trả lời:

```
CPU = 95%
RAM = 90%
Disk = 98%
```

→ biết **có vấn đề gì**.

**Log**:

```
database connection timeout
```

→ biết **lỗi gì xảy ra**.

**Trace**:

```
Client
 ↓
API Gateway
 ↓
Service A
 ↓
Service B
 ↓
Database
```

→ biết request **chậm ở đâu**.

Với bài tập tạo folder Tamdtt chứa dash 

đặt câu hỏi dễ hiểu trước khi làm:

1. Tôi đang muốn theo dõi cái gì?
↓
2. Dữ liệu nằm ở Data Source nào?
↓
3. Metric nào chứa dữ liệu đó?
↓
4. Label nào xác định server/device tôi cần?
↓
5. PromQL nào biến metric đó thành thông tin muốn xem?

ví dụ với label CPU trong bài tập:

Tôi muốn xem CPU
↓
Data Source = Prometheus
↓
Metric = node_cpu_seconds_total
↓
instance =  “ ”
↓
PromQL tính % CPU
↓
Grafana Panel

kết quả cấu trúc bài tập:

Grafana
└── Folder: Tamdtt
└── Dashboard: Linux Server Overview (khung chứa các panel)
├── Panel 1: CPU  #máy hiện đang sử dụng bao nhiêu CPU?
├── Panel 2: Memory #máy đang sử dụng bao nhiêu RAM?
├── Panel 3: Disk   #ổ đĩa sử dụng bao nhiêu %?
└── Panel 4: Network    #má yddang gửi nhận bao nhiêu dữ liệu mạng

luồng là 

Linux Server
│
│ CPU
│ RAM
│ Disk
│ Network
│
▼
OpenTelemetry Collector 
│
│ thu thập / chuyển metric
▼
Prometheus  (kh ochuwas dữ liệu metrix)
│
│ Grafana query
▼
Linux Server Overview