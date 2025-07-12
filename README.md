# SmartLight - Hệ Thống Đèn Thông Minh

Dự án mô phỏng hệ thống đèn thông minh với khả năng điều khiển từ xa, thống kê thời gian học tập và tích hợp với Google Assistant. Hệ thống giao tiếp giữa Arduino, Proteus và Firebase thông qua Script Python.

## Tính Năng Chính

- **Điều khiển đèn thông minh**: Bật/tắt, điều chỉnh độ sáng, chuyển đổi màu sáng
- **Chế độ tự động**: Tự động điều chỉnh độ sáng theo môi trường
- **Promodoro Timer**: Hẹn giờ học tập với chế độ học/nghỉ
- **Thống kê thời gian**: Theo dõi thời gian học tập và độ sáng trung bình
- **Ứng dụng di động**: Điều khiển từ xa qua app Flutter
- **Google Assistant**: Điều khiển bằng giọng nói
- **Biểu đồ thống kê**: Hiển thị dữ liệu thời gian học và độ sáng

---

## Cấu Trúc Thư Mục

```
iot_smartlight/
├── Arduino/                    # Code Arduino và file .hex
│   ├── nhom08/
│   │   └── nhom08.ino           # Mã nguồn Arduino chính
│   └── nhom08.ino.hex           # File hex đã biên dịch
├── Proteus/                   # Mạch mô phỏng
│   └── Nhom08/
│       └── Nhom08.pdsprj        # File mô phỏng Proteus
├── Python_IoT/               # Script Python giao tiếp
│   ├── Portable/                # Môi trường Python portable
│   └── Python/
│       ├── iot.py              # Script chính
│       └── requirements.txt    # Thư viện cần thiết
├── Flutter/                  # Ứng dụng di động
│   ├── app-release.apk         # File APK đã biên dịch
│   └── source/                 # Mã nguồn Flutter
├── NodeJs/                   # Máy chủ thông báo
│   ├── gg assistant.zip        # Google Assistant server
│   └── Nodejs_Sever_Notification.zip # Notification server
├── Firebase/                 # Dữ liệu Firebase export
│   └── iot-nhom08-default-rtdb-export.json # Backup dữ liệu Firebase
└── vspd.exe                 # Phần mềm tạo cổng COM ảo
```

---

## Kiến Trúc Hệ Thống

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Flutter   │    │   NodeJs    │    │  Google     │
│     App     │    │   Server    │    │ Assistant   │
└──────┬──────┘    └──────┬──────┘    └──────┬──────┘
       │                  │                   │
       └──────────────────┼───────────────────┘
                          │
                    ┌─────▼─────┐
                    │  Firebase │
                    │ (iot-nhom08) │
                    └─────┬─────┘
                          │
                    ┌─────▼─────┐
                    │  Python   │
                    │   Script  │
                    └─────┬─────┘
                          │
                    ┌─────▼─────┐
                    │  Arduino  │
                    │   + LDR   │
                    └───────────┘
```

---

## Thiết Lập Ban Đầu

### 1. Cài đặt vspd.exe
- Sử dụng phần mềm này để thiết lập liên kết ảo giữa các cổng COM
- Tạo kết nối COM1 với COM4 để tạo kết nối ảo giữa hai cổng

### 2. Cấu hình Firebase
Dự án sử dụng Firebase project chính:

#### Firebase Project:
- **URL**: `https://iot-nhom08-default-rtdb.asia-southeast1.firebasedatabase.app`
- **Project ID**: `iot-nhom08`

> **Lưu ý**: Flutter app hiện tại đang cấu hình với project `light-study` nhưng cần được cập nhật để sử dụng project `iot-nhom08` để đồng bộ với Python script.

#### Cấu hình các node chính:
- `/LED_CONTROL` - Điều khiển đèn
- `/TIME_USE` - Thống kê thời gian học
- `/LIGHT_INTENSITY` - Thống kê độ sáng
- `/PROMODORO` - Cài đặt Promodoro
- `/TIME_NOTIFICATION` - Cấu hình thông báo (tokens, thời gian)

### 3. Cấu hình cổng COM
- Mở Device Manager
- Kiểm tra cổng COM4 có sẵn
- Cấu hình baud rate: 9600

---

## Chạy Ứng Dụng Python

### Cách 1: Dùng Môi Trường Python Portable

1. Vào thư mục **Python_IoT/Portable**
2. Chạy **WinPython Command Prompt.exe** hoặc **WinPython Powershell Prompt.exe**
3. Nhập lệnh:
```bash
python iot.py
```

### Cách 2: Tự Cài Đặt Python

1. Cài đặt Python trên máy tính
2. Vào thư mục **Python_IoT/Python**
3. Cài đặt thư viện:
```bash
pip install -r requirements.txt
```
4. Chạy ứng dụng:
```bash
python iot.py
```

---

## Mô Phỏng Trên Proteus

1. Mở file mô phỏng trong thư mục **Proteus/Nhom08**
2. Nạp file `.hex` vào Arduino Uno R3
3. Chạy mô phỏng để kiểm tra hoạt động

---

## Nạp Chương Trình Arduino

### Cách 1: Sử dụng file .hex
- Sử dụng file `nhom08.ino.hex` trong thư mục **Arduino**
- Nạp trực tiếp vào Uno R3 trong mô phỏng Proteus

### Cách 2: Biên dịch từ mã nguồn
1. Mở `nhom08.ino` trong Arduino IDE
2. Biên dịch và nạp chương trình

---

## Chạy Code NodeJs

### 1. Cài đặt NodeJs
- Tải và cài đặt NodeJs từ [nodejs.org](https://nodejs.org)

### 2. Giải nén và chạy server thông báo
```bash
# Giải nén Nodejs_Sever_Notification.zip
cd Nodejs_Sever_Notification
npm install
node sever_notifi.js
```

### 3. Chạy Google Assistant server
```bash
# Giải nén gg_assistant.zip
cd gg_assistant
npm install
node sever.js
```

---

## Cài Đặt App SmartLight

1. Tìm file `app-release.apk` trong thư mục **Flutter**
2. Cài đặt lên điện thoại Android
3. Mở app và cấp quyền thông báo (nếu được hỏi)

---

## Giao Thức Giao Tiếp

### Định dạng dữ liệu Serial:

#### Từ Arduino đến Python:
- **D***power*color*auto*brightness* - Dữ liệu điều khiển đèn
- **T***time* - Thời gian học tập (milliseconds)
- **I***intensity* - Cường độ sáng trung bình

#### Từ Python đến Arduino:
- **power*color*auto*brightness*** - Lệnh điều khiển đèn
- **P*state*studyTime*breakTime*** - Cài đặt Promodoro

### Ví dụ:
```
D*1*1*0*128*    # Đèn bật, màu trắng, tự động tắt, độ sáng 128
T*1500000*       # Thời gian học: 25 phút
I*156*           # Cường độ sáng trung bình: 156
P*1*25*5*        # Promodoro: bật, học 25p, nghỉ 5p
```

---

## Cấu Hinh Chi Tiết

### Arduino Pin Configuration:
- `A1` - Cảm biến ánh sáng (LDR)
- `A0` - Biến trở điều chỉnh nguồn
- `12` - Nút chuyển đổi màu đèn
- `13` - Nút chế độ tự động sáng
- `9` - LED trắng
- `10` - LED vàng
- `A4` - LED chỉ báo chế độ tự động

### Firebase Database Structure:

#### Firebase Database (iot-nhom08):
```json
{
  "LED_CONTROL": {
    "nutNguon": "1",
    "nutDoiMau": "1", 
    "nutTuDongSang": "0",
    "doSangCuaDen": "128"
  },
  "TIME_USE": {
    "2025-03-12": 22,
    "2025-03-13": 63,
    "2025-03-14": 260
  },
  "LIGHT_INTENSITY": {
    "2025-03-12": 180,
    "2025-03-13": 190,
    "2025-03-14": 210
  },
  "PROMODORO": {
    "state": "1",
    "thoiGianLam": "25",
    "thoiGianNghi": "5"
  },
  "TIME_NOTIFICATION": {
    "state_notification": "0",
    "time_notification": "16:03",
    "tokens_device": {
      "-OMAmUxzc12LiA_3Jcfg": {
        "token": "fw7DL_ywTIiK9I5xOQBybw:APA91bEAJB14WmvhlWtqWKDtpef7as3VIMRbQtqGNn-3bn0Y1DoKGeLdANJ-elkhP_bXtMjsa48Gu9YJLL5HDayDIrNryzuFVijLKvoKLA89rxr2Xk9fhWA"
      }
    }
  }
}
```

---

## Hướng Dẫn Sử Dụng

### 1. Khởi động hệ thống:
1. Chạy `vspd.exe` và tạo kết nối COM1-COM4
2. Mở mô phỏng Proteus
3. Chạy script Python: `python iot.py`
4. Khởi động NodeJs servers
5. Cài đặt và mở app Flutter

### 2. Điều khiển đèn:
- **App Flutter**: Điều khiển từ xa qua giao diện
- **Google Assistant**: "Hey Google, turn on the light"
- **Biến trở**: Điều chỉnh độ sáng trực tiếp
- **Nút vật lý**: Chuyển đổi chế độ và màu sắc

### 3. Thông báo và cài đặt:
- **Push Notification**: Nhận thông báo từ NodeJs server
- **Token Management**: Quản lý device tokens cho thông báo
- **Time Notification**: Cài đặt thời gian nhận thông báo

### 4. Theo dõi thống kê:
- Xem biểu đồ thời gian học trong app
- Kiểm tra độ sáng trung bình
- Theo dõi hiệu suất học tập

---

## Tóm Tắt

- **Arduino**: Code điều khiển đèn thông minh với cảm biến LDR
- **Proteus**: Mô phỏng mạch điện hoàn chỉnh
- **Python**: Script giao tiếp giữa Arduino và Firebase
- **Flutter**: App di động điều khiển từ xa
- **NodeJs**: Máy chủ thông báo và Google Assistant
- **vspd.exe**: Tạo cổng COM ảo cho giao tiếp
- **Firebase**: Cơ sở dữ liệu thời gian thực

---

## Hỗ Trợ

Nếu gặp vấn đề, hãy kiểm tra:
1. Kết nối cổng COM
2. Cấu hình Firebase (đảm bảo Flutter app sử dụng đúng project `iot-nhom08`)
3. Cài đặt thư viện Python
4. Quyền truy cập app

### Cập nhật Flutter App Firebase:
Để đồng bộ Flutter app với Python script, cần cập nhật:
1. File `firebase_options.dart` - thay đổi URL thành `https://iot-nhom08-default-rtdb.asia-southeast1.firebasedatabase.app`
2. File `google-services.json` - cập nhật project ID thành `iot-nhom08`
3. File `firebase.json` - cập nhật cấu hình project

---

**Dự án SmartLight - Hệ thống đèn thông minh cho học tập hiệu quả!**