youbute 영상:
https://www.youtube.com/watch?v=Wld4w16fotY

# PWM With LED Control Using RC Receiver (RD9S) and Arduino

본 프로젝트는 Radiolink RD9S RC 수신기에서 PWM 신호를 받아 아두이노를 통해 LED를 제어하는 방법을 구현합니다.  
아래의 회로도, 송신기 사진 및 유튜브 영상 참고와 함께 자세한 설명과 코드가 포함되어 있습니다.

---

## 회로도

![회로도](https://github.com/user-attachments/assets/c00dfb31-f525-4984-bd1d-f9002a9bd4a9)

---

## 송신기 사진

![송신기](https://github.com/user-attachments/assets/0e998258-6e91-4d66-a7db-3ed682ba79df)

---

## 1. 전원 & 접지

- **5V (아두이노 5V)** → 수신기 VCC, LED 모듈 공통 애노드(+5V)  
- **GND (아두이노 GND)** → 수신기 GND, LED 모듈 GND  

---

## 2. RC 수신기 → 아두이노 입력 핀 (풀업)

| RC 채널 | 입력 용도       | Arduino 핀 |
|---------|------------------|------------|
| CH8     | On/Off 스위치    | D10        |
| CH6     | 밝기 조절        | D11        |
| CH7     | 색상 제어(Hue)   | D12        |

---

## 3. LED 제어 핀

| LED 종류           | Arduino 핀 | 메모                                              |
|--------------------|-------------|---------------------------------------------------|
| On/Off 전용        | D2          | 스위치(CH8) 상태 표시                             |
| 밝기 전용 (PWM)    | D3          | CH6 → `map(1000~2000μs → 0~255)`                   |
| RGB Red (PWM)      | D5          | CH7 → Hue 변환 후 Red 채널                        |
| RGB Green (PWM)    | D6          | CH7 → Hue 변환 후 Green 채널                      |
| RGB Blue (PWM)     | D9          | CH7 → Hue 변환 후 Blue 채널                       |



## 기능 설명

- RC 수신기 RD9S의 서보 커넥터에서 채널 8, 6, 7 신호를 각각 디지털 입력으로 받아 핀 변경 인터럽트(Pin Change Interrupt)를 사용해 PWM 펄스폭을 측정합니다.
- 채널 8은 On/Off 스위치 역할로, 이 신호에 따라 D2 핀에 연결된 단색 LED가 켜지거나 꺼집니다.
- 채널 6의 펄스폭을 아두이노에서 PWM 밝기 값(0~255)으로 변환해 D3 핀에 연결된 LED 밝기를 조절합니다.
- 채널 7 신호는 HSV 색상환의 Hue 값(0~360도)으로 매핑되며, HSV→RGB 변환 후 공통 애노드 RGB LED의 색상을 제어합니다.
- HSV → RGB 변환은 Hue 값에 따라 RGB 색상을 계산하고, 공통 애노드 특성상 PWM 신호를 반전시켜 출력합니다.

---

