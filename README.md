youbute 영상:
https://www.youtube.com/watch?v=Wld4w16fotY

PWM With LED Control Using RC Receiver (RD9S) and Arduino
프로젝트 개요
이 프로젝트는 Radiolink RC 수신기(RD9S)에서 나오는 PWM 신호를 아두이노로 읽어서, 여러 가지 LED를 제어하는 시스템입니다.

채널 8은 On/Off 스위치 역할로 단색 LED 점멸에 사용

채널 6은 밝기 조절용 포텐셔미터 신호로 단색 LED 밝기 제어

채널 7은 색상 조절용 포텐셔미터 신호로 RGB LED 색상 변경에 사용

하드웨어 구성
1) RC 수신기 핀 연결
수신기 채널	기능	아두이노 핀 (입력)
채널 8 (Switch A)	On/Off 스위치	D10
채널 6 (VrA)	밝기 조절 신호	D11
채널 7 (VrB)	색상 조절 신호	D12

2) LED 출력 연결
LED 종류	기능	아두이노 핀 (출력)
단색 LED (On/Off)	스위치 상태 표시	D2
단색 LED (PWM)	밝기 조절용	D3
RGB LED (공통 애노드)	색상 표현	R: D5, G: D6, B: D9

참고: RGB LED는 공통 애노드 방식이므로 PWM 출력 신호를 반전하여 사용합니다.

![회로도-1](https://github.com/user-attachments/assets/c00dfb31-f525-4984-bd1d-f9002a9bd4a9)

동작 원리
PWM 신호 읽기
RC 수신기 각 채널은 1000~2000μs 범위의 PWM 신호를 출력합니다.
아두이노는 핀 변경 인터럽트(Pin Change Interrupt)를 이용해 상승 및 하강 엣지를 감지하여 신호의 펄스폭을 측정합니다.

채널별 기능

채널 8: 신호가 1500μs 이상일 때 스위치 ON으로 판단하여 단색 LED를 켬

채널 6: 펄스폭을 0~255 PWM 값으로 변환하여 밝기 조절 LED 출력

채널 7: 펄스폭을 0~360도의 Hue 값으로 매핑하여 HSV → RGB 변환 후 RGB LED 색상 출력

HSV to RGB 변환
색상 조절을 위해 Hue(0~360도), Saturation(채도), Value(명도)를 RGB 신호로 변환하는 함수가 구현되어 있습니다.
이 프로젝트에서는 항상 채도 1, 명도 1로 최대 밝기 색상을 표현합니다.

PWM 반전 처리
공통 애노드 RGB LED 특성상, PWM 출력 값은 255에서 계산된 RGB 값을 빼서 출력합니다.

시리얼 모니터 출력
디버깅 용도로 각 채널의 펄스폭과 밝기, Hue 값을 주기적으로 출력합니다.

![송신기](https://github.com/user-attachments/assets/0e998258-6e91-4d66-a7db-3ed682ba79df)
