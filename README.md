# loa-su-dung-arduino

# 1. Linh kiện cần chuẩn bị

* **Arduino Uno R3** 
* **Module thẻ nhớ MicroSD** 
* **Thẻ nhớ MicroSD** 
* **Loa nhỏ (8 Ohm, 0.5W - 3W)**.
* **Transistor NPN
* **Điện trở 1k Ohm**.
* **Dây nhảy Male to male
* breadboard

---

## 2. Sơ đồ đấu nối



* **VCC** -> 5V
* **GND** -> GND
* **MISO** -> Pin 12
* **MOSI** -> Pin 11
* **SCK** -> Pin 13
* **CS** -> Pin 4 (có thể thay đổi trong code)

### Kết nối Loa (Sử dụng Transistor):

Để bảo vệ Arduino không bị quá tải dòng điện, chúng ta nối qua một Transistor:

* **Chân B (Base)** của Transistor nối với **Pin 9** của Arduino qua điện trở 1k.
* **Chân C (Collector)** nối với một cực của loa. Cực còn lại của loa nối với **5V**.
* **Chân E (Emitter)** nối với **GND**.

---

## 3. Chuẩn bị file nhạc

Arduino không thể đọc trực tiếp file `.mp3`. Bạn cần chuyển đổi nhạc sang định dạng `.wav` với thông số sau:

* **Samples Rate:** 16000Hz (hoặc 8000Hz).
* **Channel:** Mono.
* **Resolution:** 8-bit.
* **Tên file:** Ngắn gọn (ví dụ: `music.wav`).

---

## 4. Lập trình (Code)

cài đặt thư viện **TMRpcm** 

```cpp
#include <SD.h>                      // Thư viện thẻ nhớ
#include <TMRpcm.h>                  // Thư viện phát nhạc
#include <SPI.h>

#define SD_ChipSelectPin 4           // Chân CS của module SD

TMRpcm audio;                        // Tạo đối tượng audio

void setup() {
  audio.speakerPin = 9;              // Chân xuất âm thanh (phải là chân PWM)

  Serial.begin(9600);
  
  if (!SD.begin(SD_ChipSelectPin)) { // Kiểm tra thẻ nhớ
    Serial.println("SD fail");
    return;
  }

  audio.setVolume(5);                // Âm lượng (từ 0 đến 7)
  audio.play("music.wav");           // Tên file nhạc trên thẻ nhớ
}

void loop() {
  // Bạn có thể thêm nút bấm Play/Pause ở đây
}


