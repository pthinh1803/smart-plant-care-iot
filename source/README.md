# Hướng Dẫn Thư Mục Source

Thư mục `source/` dùng để chứa mã nguồn firmware ESP32, dashboard và các file
cấu hình mẫu khi dự án được public lên GitHub. Hiện tại thư mục này chỉ có tài
liệu hướng dẫn để mô tả cách tổ chức source code an toàn, tránh đưa nhầm thông
tin riêng tư lên repository.

## Cấu Trúc Source Đề Xuất

```text
source/
|-- firmware/
|   |-- src/
|   |   |-- main.ino
|   |   |-- sensors.h
|   |   |-- firebase_client.h
|   |   |-- actuators.h
|   |   `-- lcd_display.h
|   |-- config.example.h
|   `-- README.md
|-- dashboard/
|   |-- src/
|   |-- public/
|   |-- .env.example
|   `-- README.md
`-- README.md
```

Nếu chưa có mã nguồn đầy đủ để public, có thể chỉ đưa tài liệu mô tả và file
config mẫu. Không nên đưa file cấu hình thật hoặc ảnh chụp có chứa thông tin
riêng tư.

## Firmware ESP32

Firmware nên được chia thành các phần rõ ràng:

- **Đọc cảm biến:** DHT11, LDR, cảm biến độ ẩm đất và pH mô phỏng.
- **Xử lý dữ liệu:** quy đổi ADC sang phần trăm độ ẩm đất, tính giá trị hiển
  thị, lọc nhiễu cơ bản nếu cần.
- **Đồng bộ Firebase:** gửi dữ liệu cảm biến, đọc chế độ, đọc ngưỡng và đọc
  lệnh thủ công.
- **Điều khiển relay:** bật/tắt quạt, máy bơm và đèn.
- **Hiển thị LCD:** luân phiên các trang trạng thái.
- **Cấu hình:** WiFi, Firebase URL, API key hoặc token phải nằm trong file mẫu
  và được thay bằng biến placeholder khi public.

## Chu Kỳ Xử Lý Nên Giữ

| Tác vụ | Chu kỳ | Ghi chú |
|---|---:|---|
| Đọc cảm biến | 2000 ms | Tránh đọc quá dày gây nhiễu và khó quan sát |
| Đọc Firebase | 1000 ms | Giúp dashboard điều khiển phản hồi tương đối nhanh |
| Gửi Firebase | 3000 ms | Giảm số lần ghi nhưng vẫn đủ dữ liệu cho biểu đồ |
| Chuyển trang LCD | 4000 ms | Đủ thời gian để đọc từng trang LCD |

## Dashboard

Dashboard nên có các khu vực chính:

- Card hiển thị nhiệt độ, độ ẩm không khí, độ ẩm đất, pH và ánh sáng.
- Biểu đồ theo thời gian cho từng thông số.
- Công tắc hoặc nút điều khiển quạt, bơm và đèn.
- Tùy chọn Auto/Manual cho từng thiết bị.
- Bảng dữ liệu hoặc chức năng xuất CSV.
- Trạng thái kết nối Firebase hoặc thời điểm cập nhật cuối.

## File Cấu Hình Mẫu

Ví dụ nội dung nên có trong `config.example.h`:

```cpp
#define IOT_WIFI_SSID "YOUR_WIFI_SSID"
#define IOT_WIFI_PASS "YOUR_WIFI_PASSWORD"

#define IOT_FIREBASE_URL "https://your-project.firebaseio.com"
#define IOT_FIREBASE_TOKEN "YOUR_FIREBASE_TOKEN"
```

Khi dùng thật, tạo file cấu hình riêng như `config.h` và đưa file đó vào
`.gitignore`.

## Quy Tắc Không Public Credential

Không commit các nội dung sau:

- WiFi SSID/password thật.
- Firebase database URL thật nếu không muốn công khai.
- API key, auth token, service account JSON.
- File `.env`, `config.h`, `secrets.h` có thông tin thật.
- Ảnh chụp màn hình IDE hoặc dashboard có lộ URL/token.

Nên commit:

- `config.example.h` hoặc `.env.example`.
- README hướng dẫn cấu hình.
- Mã nguồn đã thay credential bằng placeholder.
- Ghi chú rõ các thư viện cần cài đặt.

## Thư Viện Gợi Ý

Với firmware ESP32, các thư viện thường cần:

- WiFi cho ESP32.
- HTTPClient hoặc Firebase client phù hợp.
- DHT sensor library.
- LiquidCrystal I2C cho LCD.
- Thư viện JSON nếu cần xử lý dữ liệu phức tạp.

Với dashboard, có thể dùng HTML/CSS/JavaScript thuần hoặc framework như React,
Vite tùy mức độ hoàn thiện của dự án.

## Checklist Trước Khi Public

- Đã thay toàn bộ credential bằng placeholder.
- Đã thêm file cấu hình thật vào `.gitignore`.
- Đã kiểm tra ảnh chụp không lộ thông tin riêng tư.
- README có hướng dẫn cách tạo file cấu hình.
- Source có cấu trúc rõ ràng để người khác đọc và chạy thử.
