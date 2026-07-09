# Báo Cáo Public: IoT Plant Care Monitoring System

## 1. Giới Thiệu

Đề tài xây dựng một mô hình IoT chăm sóc cây trồng sử dụng ESP32, cảm biến môi
trường, Firebase Realtime Database, dashboard web, LCD I2C và các thiết bị chấp
hành. Hệ thống có nhiệm vụ theo dõi nhiệt độ, độ ẩm không khí, độ ẩm đất, ánh
sáng và giá trị pH mô phỏng; đồng thời hỗ trợ điều khiển quạt, máy bơm và đèn
theo chế độ tự động hoặc thủ công.

Bản báo cáo này là phiên bản public để đưa lên GitHub. Nội dung đã được viết lại
từ báo cáo gốc, chỉ giữ phần kỹ thuật và kết quả thực nghiệm cần thiết. Các
thông tin cá nhân, file Word/PDF gốc, thông tin WiFi và cấu hình Firebase thật
không được đưa vào.

## 2. Lý Do Chọn Đề Tài

Việc chăm sóc cây trồng trong nhà hoặc mô hình quy mô nhỏ thường gặp khó khăn
vì người trồng khó theo dõi liên tục các yếu tố như nhiệt độ, độ ẩm đất, độ ẩm
không khí và ánh sáng. Tưới nước thủ công có thể làm đất quá khô hoặc quá ướt,
ảnh hưởng đến sự phát triển của cây.

IoT giúp tự động hóa quá trình thu thập dữ liệu, hiển thị thông tin từ xa và
đưa ra quyết định điều khiển dựa trên dữ liệu môi trường. ESP32 được chọn vì có
WiFi tích hợp, chi phí thấp, dễ lập trình và phù hợp với các mô hình học tập.

## 3. Mục Tiêu

- Thiết kế mô hình IoT giám sát môi trường cây trồng.
- Sử dụng ESP32 làm bộ xử lý trung tâm.
- Đọc dữ liệu từ DHT11, LDR, cảm biến độ ẩm đất và pH mô phỏng.
- Hiển thị thông tin tại chỗ bằng LCD I2C 16x2.
- Gửi dữ liệu lên Firebase Realtime Database.
- Xây dựng dashboard để hiển thị số liệu, biểu đồ và điều khiển thiết bị.
- Điều khiển quạt, máy bơm và đèn theo chế độ Auto/Manual.
- Kiểm thử hệ thống trong thời gian dài và đánh giá kết quả.

## 4. Phạm Vi Thực Hiện

| Hạng mục | Nội dung |
|---|---|
| Đối tượng | Chậu cây, mô hình cây trồng trong nhà hoặc mô hình học tập |
| Quy mô | Prototype quy mô nhỏ |
| Bộ điều khiển | ESP32 |
| Dữ liệu đo | Nhiệt độ, độ ẩm không khí, độ ẩm đất, ánh sáng, pH mô phỏng |
| Lưu trữ | Firebase Realtime Database |
| Giao diện | Dashboard web |
| Chấp hành | Quạt, máy bơm, đèn |

## 5. Kiến Trúc Hệ Thống

![Sơ đồ khối hệ thống](../images/system-block-diagram.png)

Hệ thống gồm các khối chính:

- **Cảm biến:** thu thập dữ liệu môi trường.
- **ESP32:** đọc dữ liệu, xử lý, điều khiển relay và đồng bộ Firebase.
- **Firebase:** lưu dữ liệu cảm biến, lệnh điều khiển, trạng thái và ngưỡng.
- **Dashboard:** hiển thị thông tin và gửi lệnh điều khiển.
- **Thiết bị chấp hành:** quạt, máy bơm và đèn.

Luồng dữ liệu của hệ thống được thiết kế hai chiều. ESP32 gửi dữ liệu cảm biến
lên Firebase; dashboard đọc dữ liệu để hiển thị. Ngược lại, dashboard ghi lệnh
điều khiển hoặc thay đổi chế độ lên Firebase; ESP32 đọc lại và cập nhật relay.

![Sơ đồ luồng dữ liệu và điều khiển](../images/iot-data-flow.png)

## 6. Thiết Kế Phần Cứng

![Sơ đồ nguyên lý phần cứng](../images/hardware-schematic.png)

| Thiết bị | Chức năng |
|---|---|
| ESP32 | Điều khiển trung tâm và kết nối WiFi |
| DHT11 | Đo nhiệt độ và độ ẩm không khí |
| LDR | Nhận biết điều kiện sáng/tối |
| Cảm biến độ ẩm đất | Đo độ ẩm đất qua tín hiệu analog |
| LCD I2C | Hiển thị trạng thái tại chỗ |
| Relay | Đóng/ngắt quạt, bơm và đèn |
| Quạt | Làm mát khi nhiệt độ cao |
| Máy bơm | Tưới nước khi đất khô |
| Đèn | Bổ sung ánh sáng khi thiếu sáng |

![Mô hình phần cứng hoàn thiện](../images/hardware-prototype.png)

## 7. Bảng Kết Nối

| STT | Thiết bị / Module | Chân tín hiệu | Chân ESP32 | Kiểu tín hiệu | Chức năng |
|---:|---|---|---|---|---|
| 1 | DHT11 | DATA | GPIO23 | Digital Input | Đọc nhiệt độ, độ ẩm không khí |
| 2 | LDR | DO | GPIO34 | Digital Input | Đọc trạng thái sáng/tối |
| 3 | Cảm biến độ ẩm đất | AO | GPIO35 | Analog Input | Đọc ADC độ ẩm đất |
| 4 | Relay quạt | IN | GPIO26 | Digital Output | Bật/tắt quạt |
| 5 | Relay bơm | IN | GPIO27 | Digital Output | Bật/tắt máy bơm |
| 6 | Relay đèn | IN | GPIO17 | Digital Output | Bật/tắt đèn |
| 7 | LCD I2C | SDA | GPIO21 | I2C Data | Truyền dữ liệu LCD |
| 8 | LCD I2C | SCL | GPIO22 | I2C Clock | Xung clock I2C |

## 8. Thiết Kế Phần Mềm

Firmware ESP32 được tổ chức theo các tác vụ có chu kỳ riêng:

| Tác vụ | Chu kỳ | Nội dung |
|---|---:|---|
| Đọc cảm biến | 2000 ms | Đọc DHT11, LDR, độ ẩm đất và pH mô phỏng |
| Đọc Firebase | 1000 ms | Cập nhật chế độ, ngưỡng và lệnh điều khiển |
| Gửi Firebase | 3000 ms | Gửi dữ liệu cảm biến lên Firebase |
| Chuyển trang LCD | 4000 ms | Luân phiên nội dung hiển thị LCD |

![Lưu đồ thuật toán](../images/control-flowchart.png)

Ngưỡng điều khiển tự động:

| Thiết bị | Điều kiện bật | Ngưỡng |
|---|---|---:|
| Quạt | Nhiệt độ lớn hơn hoặc bằng ngưỡng | 32°C |
| Máy bơm | Độ ẩm đất nhỏ hơn hoặc bằng ngưỡng | 40% |
| Đèn | Ánh sáng nhỏ hơn hoặc bằng ngưỡng | 500 lux |

## 9. Firebase Và Dashboard

Firebase là lớp trung gian giữa ESP32 và dashboard. Các nhánh dữ liệu chính gồm
`sensor`, `control`, `status`, `threshold` và `mode`.

![Dữ liệu cảm biến trên Firebase](../images/firebase-sensor-data.png)

![Dữ liệu điều khiển trên Firebase](../images/firebase-control-data.png)

Dashboard hiển thị dữ liệu theo thời gian thực bằng card và biểu đồ. Người dùng
có thể theo dõi nhiệt độ, độ ẩm không khí, độ ẩm đất, pH, ánh sáng và điều
khiển quạt, bơm, đèn từ xa.

![Dashboard tổng quan](../images/dashboard-overview.png)

![Dashboard điều khiển](../images/dashboard-control.png)

## 10. Kết Quả Kiểm Thử

Hệ thống đã được kiểm tra theo các nhóm chức năng: khởi động, kết nối WiFi,
kết nối Firebase, đọc cảm biến, hiển thị LCD, truyền dữ liệu, điều khiển tự
động và điều khiển thủ công.

| Thông số | Giá trị quan sát cuối | Ngưỡng liên quan | Nhận xét |
|---|---:|---:|---|
| Nhiệt độ | 36,4°C | 32°C | Vượt ngưỡng, phù hợp điều kiện bật quạt |
| Độ ẩm không khí | 62% | Không áp dụng | Dữ liệu ổn định, dao động nhẹ |
| Độ ẩm đất | 0% | 40% | Đất khô, phù hợp điều kiện bật bơm |
| pH | 6,01 | Không áp dụng | Giá trị pH mô phỏng hiển thị được |
| Ánh sáng | 100.000 lux | 500 lux | Ánh sáng cao, phù hợp điều kiện tắt đèn |

![Biểu đồ nhiệt độ](../images/temperature-chart.png)

![Biểu đồ độ ẩm không khí](../images/humidity-chart.png)

![Biểu đồ độ ẩm đất](../images/soil-moisture-chart.png)

![Biểu đồ pH](../images/ph-chart.png)

![Biểu đồ ánh sáng](../images/light-chart.png)

## 11. Đánh Giá

Ưu điểm:

- Hệ thống có đầy đủ các khối cơ bản của một mô hình IoT.
- ESP32 giúp giảm chi phí vì đã tích hợp WiFi.
- Dashboard và LCD giúp quan sát dữ liệu ở cả tại chỗ và từ xa.
- Có thể vận hành tự động theo ngưỡng hoặc điều khiển thủ công.
- Firebase giúp đồng bộ dữ liệu nhanh và dễ kiểm thử.

Hạn chế:

- DHT11 có độ chính xác chưa cao.
- LDR đọc dạng digital nên giá trị lux chỉ mang tính quy đổi.
- Độ ẩm đất có thể dao động do vị trí cảm biến và nhiễu analog.
- pH hiện tại là giá trị mô phỏng, chưa dùng cảm biến pH thật.
- Hệ thống phụ thuộc vào WiFi và Firebase.

## 12. Hướng Phát Triển

- Thay DHT11 bằng DHT22, SHT30 hoặc BME280.
- Thay LDR bằng cảm biến BH1750 để đo lux chính xác hơn.
- Dùng cảm biến độ ẩm đất điện dung để tăng độ bền.
- Bổ sung cảm biến pH thật và quy trình hiệu chuẩn.
- Thêm cảnh báo khi thông số vượt ngưỡng.
- Hỗ trợ nhiều chậu cây hoặc nhiều khu vực trồng.
- Bổ sung chế độ offline khi mất Internet.
- Thiết kế PCB và vỏ bảo vệ chống ẩm.

## 13. Kết Luận

Đồ án đã xây dựng được mô hình IoT chăm sóc cây trồng ở mức cơ bản. Hệ thống
đọc dữ liệu cảm biến, hiển thị trên LCD, đồng bộ Firebase, hiển thị dashboard
và điều khiển thiết bị chấp hành theo ngưỡng. Phiên bản hiện tại phù hợp cho
mục tiêu học tập, trình bày đồ án và làm nền tảng phát triển các tính năng nâng
cao hơn trong tương lai.

