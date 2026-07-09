# Kết Quả Kiểm Thử Hệ Thống

Thư mục này tổng hợp kết quả kiểm thử của hệ thống IoT chăm sóc cây trồng, bao
gồm kiểm tra khởi động, cảm biến, thiết bị chấp hành, dashboard, Firebase và
quá trình vận hành dài hạn.

## 1. Kiểm Tra Khởi Động

| STT | Nội dung kiểm tra | Kết quả quan sát | Đánh giá |
|---:|---|---|---|
| 1 | Cấp nguồn cho hệ thống | ESP32 hoạt động, các module có tín hiệu | Đạt |
| 2 | Kết nối WiFi | LCD hiển thị `WiFi OK` | Đạt |
| 3 | Kết nối Firebase | LCD hiển thị `Firebase HTTP` | Đạt |
| 4 | LCD khởi động | LCD hiển thị đúng nội dung | Đạt |

Các bước khởi động cho thấy phần cứng chính, WiFi, Firebase và LCD đều hoạt
động đúng theo yêu cầu ban đầu.

## 2. Kiểm Tra Cảm Biến

| STT | Cảm biến | Nội dung kiểm tra | Kết quả |
|---:|---|---|---|
| 1 | DHT11 | Đọc nhiệt độ và độ ẩm không khí | Đọc được dữ liệu |
| 2 | LDR | Nhận biết trạng thái sáng/tối | Hoạt động đúng |
| 3 | Cảm biến độ ẩm đất | Đọc giá trị ADC và quy đổi phần trăm | Hoạt động đúng |
| 4 | pH mô phỏng | Tạo giá trị pH trong khoảng cài đặt | Hiển thị được dữ liệu |

![LCD hiển thị dữ liệu cảm biến](../images/lcd-sensor-page.png)

![LCD hiển thị ADC, pH và LDR](../images/lcd-adc-page.png)

## 3. Kiểm Tra Thiết Bị Chấp Hành

| STT | Thiết bị | Điều kiện kiểm tra | Kết quả |
|---:|---|---|---|
| 1 | Quạt làm mát | Nhiệt độ vượt ngưỡng cài đặt | Quạt bật |
| 2 | Máy bơm nước | Độ ẩm đất thấp hơn ngưỡng | Máy bơm bật |
| 3 | Đèn chiếu sáng | Môi trường thiếu sáng | Đèn bật |
| 4 | Điều khiển thủ công | Thay đổi trạng thái từ Firebase/dashboard | Thiết bị phản hồi đúng |

![LCD hiển thị trạng thái thiết bị](../images/lcd-actuator-page.png)

## 4. Kiểm Tra Firebase Và Dashboard

Firebase nhận dữ liệu cảm biến từ ESP32, đồng thời lưu lệnh điều khiển và trạng
thái thiết bị. Dashboard có thể đọc dữ liệu này để hiển thị card, biểu đồ và
khu vực điều khiển.

![Dữ liệu cảm biến trên Firebase](../images/firebase-sensor-data.png)

![Dashboard tổng quan](../images/dashboard-overview.png)

![Dashboard điều khiển thiết bị](../images/dashboard-control.png)

## 5. Kịch Bản Kiểm Thử Dài Hạn

| Giai đoạn | Thời gian | Nội dung | Kết quả mong muốn | Cách đánh giá |
|---|---|---|---|---|
| 1 | 20:00 - 20:10 | Cấp nguồn, kiểm tra ESP32 và WiFi | LCD hiển thị kết nối, dashboard nhận dữ liệu | Quan sát LCD và dashboard |
| 2 | 20:10 - 23:00 | Cho hệ thống chạy ổn định ban đầu | Dữ liệu bắt đầu xuất hiện trên biểu đồ | Quan sát đường biểu đồ |
| 3 | 23:00 - 11:58 | Duy trì vận hành liên tục | Dashboard tiếp tục cập nhật dữ liệu | Theo dõi theo trục thời gian |
| 4 | 11:58 - 15:30 | Quan sát khi môi trường thay đổi | Dữ liệu cảm biến biến thiên và được ghi nhận | So sánh dạng biểu đồ |
| 5 | Kết thúc | Tổng hợp giá trị cuối | Trạng thái thiết bị phù hợp ngưỡng | Đối chiếu giá trị với ngưỡng |

## 6. Kết Quả Tổng Hợp

| STT | Thông số kiểm thử | Giá trị quan sát cuối | Ngưỡng liên quan | Nhận xét | Đánh giá |
|---:|---|---:|---:|---|---|
| 1 | Nhiệt độ | 36,4°C | 32°C | Vượt ngưỡng, phù hợp điều kiện bật quạt | Đạt |
| 2 | Độ ẩm không khí | 62% | Không áp dụng | Dữ liệu ổn định, có dao động nhẹ | Đạt |
| 3 | Độ ẩm đất | 0% | 40% | Đất khô, phù hợp điều kiện bật máy bơm | Đạt |
| 4 | pH | 6,01 | Không áp dụng | Hiển thị được dữ liệu pH mô phỏng | Đạt |
| 5 | Ánh sáng | 100.000 lux | 500 lux | Ánh sáng cao, phù hợp điều kiện tắt đèn | Đạt |
| 6 | Dashboard | Có biểu đồ theo thời gian | Không áp dụng | Dữ liệu được cập nhật và lưu vết | Đạt |
| 7 | Firebase | Cập nhật theo chu kỳ | Không áp dụng | ESP32 gửi dữ liệu ổn định | Đạt |

## 7. Đánh Giá Điều Khiển Tự Động

| Thiết bị | Dữ liệu đầu vào | Điều kiện điều khiển | Trạng thái mong muốn | Đánh giá |
|---|---|---|---|---|
| Quạt làm mát | Nhiệt độ 36,4°C | `>= 32°C` | Quạt bật | Đạt |
| Máy bơm nước | Độ ẩm đất 0% | `<= 40%` | Máy bơm bật | Đạt |
| Đèn chiếu sáng | Ánh sáng 100.000 lux | `> 500 lux` | Đèn tắt | Đạt |

Kết quả cho thấy logic điều khiển tự động phù hợp với các giá trị cảm biến quan
sát được ở cuối quá trình kiểm thử.

## 8. Biểu Đồ Kiểm Thử

![Biểu đồ nhiệt độ](../images/temperature-chart.png)

![Biểu đồ độ ẩm không khí](../images/humidity-chart.png)

![Biểu đồ độ ẩm đất](../images/soil-moisture-chart.png)

![Biểu đồ pH mô phỏng](../images/ph-chart.png)

![Biểu đồ ánh sáng](../images/light-chart.png)

## 9. Nhận Xét

Hệ thống có thể hoạt động trong thời gian dài, cập nhật dữ liệu lên Firebase và
hiển thị trên dashboard. Các biểu đồ có dữ liệu theo thời gian, chứng minh hệ
thống đã ghi nhận được sự thay đổi của môi trường. Chức năng điều khiển tự động
cũng phù hợp với ngưỡng đã cấu hình.

Một số điểm cần cải thiện gồm độ chính xác của cảm biến, khả năng đo lux thực,
khả năng đo pH thực và độ ổn định phần cứng khi triển khai gần nước hoặc trong
môi trường có độ ẩm cao.

