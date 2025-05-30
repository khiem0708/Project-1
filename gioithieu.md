
# 📡 Hệ Thống Giám Sát Nhiệt Độ Qua Internet

## 📖 Giới Thiệu

Đây là một dự án IoT sử dụng vi điều khiển **ESP8266 NodeMCU**, cảm biến **DHT11** và nền tảng **Blynk IoT** để giám sát và điều khiển nhiệt độ, độ ẩm từ xa. Hệ thống sẽ tự động kích hoạt **quạt làm mát** và **còi báo động** khi điều kiện môi trường vượt ngưỡng cài đặt.

## 🧠 Tính Năng Chính

- Đo nhiệt độ và độ ẩm bằng cảm biến DHT11
- Hiển thị dữ liệu lên **OLED** và ứng dụng Blynk
- Cảnh báo bằng còi buzzer khi nhiệt độ hoặc độ ẩm vượt ngưỡng
- Tự động bật quạt khi nhiệt độ cao
- Giám sát và điều khiển từ xa thông qua Internet

## 🧰 Phần Cứng Sử Dụng

| Thiết bị         | Mô tả                                |
|------------------|---------------------------------------|
| ESP8266 NodeMCU | Vi điều khiển tích hợp WiFi           |
| Cảm biến DHT11  | Đo nhiệt độ và độ ẩm                  |
| Quạt DC         | Làm mát khi nhiệt độ cao              |
| Buzzer          | Cảnh báo bằng âm thanh                |
| OLED SSD1306    | Hiển thị nhiệt độ, độ ẩm và trạng thái|
| Điện trở, transistor, LED, tụ điện, diode… | Phụ kiện mạch điện |

## 🖥️ Phần Mềm Sử Dụng

- **Arduino IDE** (lập trình ESP8266)
- **Thư viện Blynk**, **DHTesp**, **Adafruit SSD1306**
- **Blynk IoT Platform** (giám sát từ xa)
- **Web Dashboard và Mobile App của Blynk**

## 🔌 Sơ Đồ Kết Nối

```
DHT11 -> D0 (GPIO16)
Buzzer -> D5 (GPIO14)
Fan    -> D6 (GPIO12)
OLED   -> I2C (SDA: D2, SCL: D1)
```

## 💻 Cách Sử Dụng

### 1. Chuẩn Bị

- Đăng ký tài khoản tại [blynk.cloud](https://blynk.cloud)
- Tạo project, thêm các **Datastreams (V1, V2, V3, V4)**

### 2. Nạp Code

- Cài đặt các thư viện cần thiết trong Arduino IDE
- Sửa thông tin WiFi và Blynk Token trong mã nguồn
- Nạp chương trình vào ESP8266

### 3. Theo Dõi và Điều Khiển

- Xem dữ liệu trực tiếp trên ứng dụng Blynk và màn hình OLED
- Khi nhiệt độ > 36°C: **Quạt sẽ quay**
- Khi độ ẩm > 60%: **Buzzer sẽ kêu**
- Có thể điều khiển thủ công quạt/buzzer từ Blynk

## 📊 Kết Quả Thử Nghiệm

- Hệ thống hoạt động ổn định trong môi trường thật
- Phản hồi nhanh, giao diện dễ sử dụng
- Có thể mở rộng với các cảm biến khác như **DHT22**, **BME280**, **MQ135**...

## 🔮 Hướng Phát Triển

- Nâng cấp cảm biến để đo chính xác hơn
- Sử dụng **ESP32** thay thế ESP8266 để có thêm tính năng
- Áp dụng thuật toán AI dự đoán xu hướng nhiệt độ
- Tạo server riêng thay vì dùng nền tảng Blynk

## 📚 Tài Liệu Tham Khảo

- [Blynk Docs](https://docs.blynk.io)
- [ESP8266 Technical Manual](https://www.espressif.com/sites/default/files/documentation/esp8266-technical_reference_en.pdf)
- [DHT11 Datasheet](https://cdn-shop.adafruit.com/datasheets/DHT11-chinese.pdf)

## 👨‍💻 Tác giả

- **Sinh viên thực hiện**: Tạ Duy Khiêm - MSSV: 2211575  
- **Giáo viên hướng dẫn**: Võ Tuấn Kiệt  
- **Trường**: Đại học Bách Khoa - ĐHQG TP.HCM  
- **Thời gian**: Tháng 3/2025
