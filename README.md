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


## 연결 정보 및 핀 배치

| 기능                  | RC 수신기 채널 | 아두이노 핀 번호  | 설명                                |
|-----------------------|----------------|------------------|-----------------------------------|
| 채널 8 (Switch A)      | 8              | D10              | On/Off 스위치 신호 입력           |
| 채널 6 (VrA)           | 6              | D11              | 밝기 조절용 아날로그 신호 입력    |
| 채널 7 (VrB)           | 7              | D12              | 색상 제어용 아날로그 신호 입력    |
| On/Off LED             | -              | D2               | 채널 8 스위치 상태에 따라 LED ON/OFF |
| 밝기 제어용 LED (PWM)  | -              | D3               | 채널 6 펄스폭에 따라 밝기 제어    |
| RGB LED (공통 애노드)  | -              | D5(Red), D6(Green), D9(Blue) | 채널 7 값에 따라 색상 제어 (HSV → RGB 변환) |

---

## 기능 설명

- RC 수신기 RD9S의 서보 커넥터에서 채널 8, 6, 7 신호를 각각 디지털 입력으로 받아 핀 변경 인터럽트(Pin Change Interrupt)를 사용해 PWM 펄스폭을 측정합니다.
- 채널 8은 On/Off 스위치 역할로, 이 신호에 따라 D2 핀에 연결된 단색 LED가 켜지거나 꺼집니다.
- 채널 6의 펄스폭을 아두이노에서 PWM 밝기 값(0~255)으로 변환해 D3 핀에 연결된 LED 밝기를 조절합니다.
- 채널 7 신호는 HSV 색상환의 Hue 값(0~360도)으로 매핑되며, HSV→RGB 변환 후 공통 애노드 RGB LED의 색상을 제어합니다.
- HSV → RGB 변환은 Hue 값에 따라 RGB 색상을 계산하고, 공통 애노드 특성상 PWM 신호를 반전시켜 출력합니다.

---

![송신기](https://github.com/user-attachments/assets/0e998258-6e91-4d66-a7db-3ed682ba79df)
