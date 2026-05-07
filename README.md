# ☀️ HELIOS-BMS

## **1. Project Summary (프로젝트 요약)**

**광원 추적형 태양광 충전 및 다중 센서 안전 제어를 통합한 차세대 EV 플랫폼**

자동차 산업의 전동화 및 SDV(Software Defined Vehicle) 전환 흐름에 대응하여, **태양광 에너지 수집, 고효율 충전 시스템, 배터리 안전 관리, 차량 주행 제어** 기능을 단일 MCU 플랫폼으로 제작



## 2. Key Features (주요 기능)


### 🔋 Buck Converter 기반 태양광 충전 시스템

- **Buck Converter 기반 전용 충전 모듈** 설계로 태양광 전력을 배터리 충전 전압으로 변환
- 태양전지–배터리 결합 시스템의 비선형 동특성을 반영한 **4차 소신호 모델링** 수행
- **MPPT–CC–CV 통합 충전 제어** 구조로 입력 조건과 배터리 상태에 따른 자동 모드 전환
- **PI 제어기 이중 루프 구조** : 전류 루프 1.5 kHz 교차 / 전압 루프 1 Hz 교차로 루프 간 간섭 방지
- 비선형 시뮬레이션 및 실제 STM32 구현을 통해 **평균 약 81~82 % 효율** 달성 (상용 MPPT 모듈 약 78 % 대비 향상)

### 🛡️ 다중 센서 기반 배터리 안전 관리 시스템 (BMS)

- **온도(NTC Thermistor)·가스(MQ-135)·전압·전류(INA219, I2C)** 4종 센서 기반 실시간 계측
- 기존 BMS 의 한계인 **열폭주 초기 오프개싱(Off-gassing) 현상** 을 가스 센서로 조기 감지
- 센서별 **SAFE / WARNING / DANGER** 3단계 상태 분류 및 Electrical 그룹 통합 판단
- WARNING 개수에 따른 **단계적 속도 제한 (80 % → 60 % → 40 % → 0 %)** 및 ramp 방식 점진 반영
- **DANGER latch 구조** 를 통한 이상 원인 기록 및 UART DMA 기반 1초 주기 로그 출력

### 🚗 차량 주행 제어 시스템 (Manual / Auto)

- **수동 주행 모드** : Bluetooth(HC-05) + STM32F411 Blackpill 조이스틱 기반 1바이트 ASCII 명령 제어
- **자율 주행 모드** : 3채널 HC-SR04 초음파 센서 + 4-state FSM(SCAN / PIVOT / BACK / STOP)
- TIM3 입력 캡처를 이용한 ECHO 펄스 측정 및 거리 환산 ($Distance = EchoPulse / 58$)
- **L298N 모터 드라이버** 기반 PWM 속도 제어 및 좌·우 센서 거리 비교를 통한 회피 방향 결정

### 📡 CAN 통신 기반 분산 제어

- **Master Board ↔ Slave Board** 간 CAN 통신을 통한 기능 분산
- **MCP2515 + TJA1050** 구성, 8 MHz 크리스탈 기준 **125 kbps** 비트레이트
- 비트 타이밍 세그먼트(Sync / Prop / Phase1 / Phase2 = 1 / 3 / 8 / 4 Tq) 설계 및 **샘플 포인트 75 %** 적용

### ☀️ 광원 추적 시스템 (Solar Tracking)

- **4채널 CDS 조도 센서** 의 상대 비교 방식 기반 태양 방향 추정
- 좌우 센서 차이(`error_x`)로 **Pan**, 상하 센서 차이(`error_y`)로 **Tilt** 결정
- **Pan/Tilt 2축 서보 모터(MG996R)** 와 PWM 제어를 통한 패널 각도 실시간 추적
- 센서 출력단에 **저역통과 필터(LPF, fc ≒ 15.9 Hz)** 와 **OP-AMP 버퍼(LM358N)** 적용으로 노이즈 제거 및 임피던스 매칭

---

## 🛠 3. Tech Stack (기술 스택)

### 3.1 Language (사용 언어)

![C](https://img.shields.io/badge/C-00599C?style=for-the-badge&logo=c&logoColor=white)![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)

### 3.2 Development Environment (개발 환경)

| **STM32CubeIDE** | **STM32CubeMX** |
| :---: | :---: |
| ![STM32CubeIDE](images/stm32cubeide.png) | ![STM32CubeMX](images/stm32cubemx.png) |
| IDE | Configuration |

### 3.3 Simulation & Analysis (제어 해석 및 시뮬레이션)

| **Google Colab** | **Falstad Circuit Simulator** |
| :---: | :---: |
| <img src="images/Colab.png" width="200"> | <img src="images/Falstad.png" width="200"> |
| 4차 소신호 모델 / Bode Plot 분석 | Buck Converter 회로 동작 검증 |

### 3.4 Debugging Tools (실험 및 디버깅 환경)

| **Moserial** | **Serial Bluetooth Terminal** |
| :---: | :---: |
| <img src="images/Moserial.png" width="200"> | <img src="images/Serial_Bluetooth_Terminal.png" width="200"> |
| UART 로그 모니터링 | Bluetooth 기반 명령 송수신 테스트 |

### 3.5 Collaboration Tools (협업 도구)

![Github](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)![Discord](https://img.shields.io/badge/Discord-7289DA?style=for-the-badge&logo=discord&logoColor=white)![Notion](https://img.shields.io/badge/Notion-000000?style=for-the-badge&logo=notion&logoColor=white)

---

## 📂 4. Project Structure (프로젝트 구조)

### 4.1 Project Tree (프로젝트 트리)

```
HELIOS_BMS/
├── Solar_master_R01/                # Master Board 펌웨어 (차량 제어 + BMS)
│   ├── Core/
│   │   ├── Inc/                     # 헤더 파일 (.h)
│   │   └── Src/
│   │       ├── main.c               # 시스템 초기화 및 메인 루프
│   │       ├── app_bms.c            # BMS 통합 태스크 (App_Bms_Task)
│   │       ├── bms_sensor.c         # INA219 / NTC / MQ-135 센서 측정
│   │       ├── bms_safety_manager.c # SAFE / WARNING / DANGER 상태 판단
│   │       ├── safe_drive.c         # 단계적 속도 제한 (ramp 적용)
│   │       ├── car.c                # Manual / Auto 주행 FSM
│   │       ├── ultrasonic.c         # HC-SR04 거리 측정 (TIM3 입력 캡처)
│   │       ├── can.c                # MCP2515 CAN 송수신 (SPI 기반)
│   │       └── bms_message.c        # UART6 DMA 기반 로그 출력
│   └── Solar_master_R01.ioc         # STM32CubeMX 설정 파일
│
├── Solar_slave_R01/                 # Slave Board 펌웨어 (태양광 + 충전)
│   ├── Core/
│   │   ├── Inc/
│   │   └── Src/
│   │       ├── main.c               # 시스템 초기화 및 메인 루프
│   │       ├── trace.c              # 4채널 CDS 기반 Pan/Tilt 제어
│   │       ├── app_charger.c        # 충전 시스템 상위 앱 (로그·주기 관리)
│   │       ├── charger_state.c      # MPPT-CC-CV 통합 충전 상태 머신
│   │       ├── solar_pi_control.c   # 전류·전압 이중 루프 PI 제어기
│   │       ├── solar_sensing.c      # 입출력 전압·전류 ADC 계측
│   │       └── can.c                # MCP2515 CAN 수신
│   └── Solar_slave_R01.ioc          # STM32CubeMX 설정 파일
│
├── Remote/                          # 원격 조종기 (STM32F411 Blackpill)
│   └── Core/
│       └── Src/
│           └── main.c               # 조이스틱 ADC + 푸쉬버튼 입력
│
├── 공모전파일/                        # 공모전 제출 자료 (보고서, 사진, BOM)
│
├── .gitignore                       # Git 제외 항목 설정
└── README.md                        # 프로젝트 개요 문서
```

### 4.2 Hardware Block Diagram (전체 하드웨어 블록도)


![HW Diagram](<images/HW Block Diagram.png>)



| **Master Board** | **Slave Board** |
| :---: | :---: |
| 차량 주행 제어 + 배터리 안전 관리 | 광원 추적 + 태양광 충전 |
| HC-05 / 조이스틱 명령 수신 | 4채널 CDS + 서보 Pan/Tilt 제어 |
| 다중 센서 기반 BMS 판단 | Buck Converter + MPPT-CC-CV |
| L298N 모터 드라이버 PWM 제어 | 입출력 전압·전류 ADC 계측 |

### 4.3 Master Board FSM

![MasterBoard](<images/Master Board FSM.png>)



### 4.4 Slave Board FSM

![SlaveBoard](<images/Slave Board FSM.png>)



---

## 🧮 5. Control Theory & Implementation (제어 이론 및 구현)

### 5.1 4차 소신호 모델링 (4th-order Small-signal Modeling)

태양전지–배터리 결합 시스템은 입력(태양전지의 비선형 I-V 특성)과 출력(배터리 CC-CV 특성)이 동시에 변하는 비선형 시스템이다. 단순 Buck Converter 모델로는 이러한 동특성을 정확히 반영할 수 없으므로, 입력과 출력의 결합 특성을 고려한 **4차 소신호 모델** 을 구성하였다.

**상태 변수 정의:**

$$x = [v_{pv}, \ v_{in}, \ i_L, \ v_o]^T$$

| 상태 변수 | 의미 |
| :---: | :--- |
| $v_{pv}$ | 태양전지 전압 |
| $v_{in}$ | 스위칭 입력 전압 |
| $i_L$ | 인덕터 전류 |
| $v_o$ | 출력 전압 |

입력단을 $v_{pv}$ 와 $v_{in}$ 으로 분리하여 태양전지의 비선형 특성과 스위칭 동작이 결합된 입력 동특성을 반영

### 5.2 MPPT–CC–CV 통합 충전 제어

전류 지령은 **CC, CV, MPPT 조건 중 가장 작은 값** 으로 결정되며, 입력 조건과 배터리 상태에 따라 충전 모드가 자동 전환

$$i_{ref} = \min(I_{CC}, \ I_{CV}, \ I_{MPPT})$$

### 5.3 PI 제어기 설계 결과

| **구분** | **fc** | **fz** | **Kp** | **Ki** | **PM** |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **전류 루프** | 1500 Hz | 300 Hz | 0.457 | 861.9 | 63.1° |
| **전압 루프** | 1 Hz | 0.2 Hz | 6.54 | 8.21 | 168.6° |

전류 루프는 내부 루프로서 빠른 전류 추종을 위해 높은 대역폭을 가지며, 전압 루프는 외부 루프로서 충분히 느리게 동작하도록 설계하여 **루프 간 간섭을 방지** 

> 위 게인 값은 소신호 모델 기반 **이론 설계값**이며, 실제 구현 시 하드웨어 특성에 맞게 튜닝하여 적용함 (Kp_I = 0.90, Ki_I = 8.00, Kp_V = 6.00, Ki_V = 3.00)

### 5.4 BMS 안전 상태 판단 기준

| **센싱 데이터** | **상태** | **기준** |
| :---: | :---: | :---: |
| **TEMP** | SAFE | < 45 ℃ |
| | WARNING | ≥ 45 ℃ |
| | DANGER | ≥ 55 ℃ |
| **GAS** | SAFE | ΔADC < 150 |
| | WARNING | ΔADC ≥ 150 |
| | DANGER | ΔADC ≥ 400 |
| **CURRENT** | SAFE | < 1700 mA |
| | WARNING | ≥ 1700 mA |
| | DANGER | ≥ 2500 mA |
| **VOLTAGE** | UNDER WARNING | ≤ 8.0 V |
| | UNDER DANGER | ≤ 5.0 V |
| | OVER WARNING | ≥ 13.0 V |
| | OVER DANGER | ≥ 13.5 V |

#### 5.4.1 단계적 속도 제한 정책

| **시스템 상태** | **최대 속도 제한** |
| :---: | :---: |
| SAFE | 80 % |
| WARNING 1개 | 60 % |
| WARNING 2개 | 40 % |
| WARNING 3개 또는 DANGER | 0 % (차량 정지) |

속도 제한 값은 **400 ms 주기로 갱신** 되며 **ramp 방식으로 점진적으로 반영**

### 5.5 수동 주행 명령 (Remote Command Map)

| **명령 코드** | **동작** | **속도** |
| :---: | :---: | :---: |
| 'F' | 전진 | 100 % |
| 'Q' | 전진 (저속) | 50 % |
| 'B' | 후진 | 100 % |
| 'W' | 후진 (저속) | 50 % |
| 'L' | 좌 회전 | 100 % |
| 'E' | 좌 회전 (저속) | 50 % |
| 'R' | 우 회전 | 100 % |
| 'T' | 우 회전 (저속) | 50 % |
| 'S' | 정지 | 0 % |
| 'A' | 자율 / 수동 모드 전환 | - |
| 'P' | 강제 정지 / 재초기화 토글 | - |

### 5.6 CAN 통신 구성

#### 5.6.1 하드웨어 구성

| **항목** | **구성** |
| :---: | :--- |
| CAN 컨트롤러 | MCP2515 |
| CAN 트랜시버 | TJA1050 |
| 인터페이스 | SPI1 (CS: PB12, INT: PC7) |
| 클럭 | 8 MHz 크리스탈 |
| 비트레이트 | 125 kbps |

#### 5.6.2 비트 타이밍 설계

CAN 은 비동기 통신이므로 노드 간 동기화를 위해 정확한 비트 타이밍 설정이 요구된다. 하나의 비트 시간 $T_{Bit}$ 은 아래 4개 세그먼트의 합으로 구성된다.

$$T_{Bit} = T_{Sync} + T_{Prop} + T_{Phase1} + T_{Phase2}$$

8 MHz 크리스탈 기준 125 kbps 를 목표 비트레이트로 설정하면 한 비트 시간은 $8\ \mu s$ 이다. BRP=1 로 설정 시 Time Quantum 은 $T_q = 0.5\ \mu s$ 이므로 한 비트당 **16 Tq** 가 필요하다.

| **세그먼트** | **설정값** | **길이** |
| :---: | :---: | :---: |
| Sync_Seg | 고정 | 1 Tq |
| Prop_Seg | PRSEG = 2 | 3 Tq |
| Phase_Seg1 | PHSEG1 = 7 | 8 Tq |
| Phase_Seg2 | PHSEG2 = 3 | 4 Tq |
| **합계** | | **16 Tq** |

샘플 포인트는 비트 시간의 **75 %** 지점으로 설정하여 버스 신호가 안정된 영역에서 샘플링을 수행한다.

$$Sample\ Point = \frac{T_{Sync} + T_{Prop} + T_{Phase1}}{T_{Bit}} = \frac{1 + 3 + 8}{16} = 75\ \%$$

MCP2515 의 비트 타이밍 레지스터 설정값은 다음과 같다.

| **레지스터** | **값** | **설명** |
| :---: | :---: | :--- |
| CNF1 | `0x01` | BRP = 1 |
| CNF2 | `0xBA` | PHSEG1 = 7, PRSEG = 2, BTLMODE = 1 |
| CNF3 | `0x03` | PHSEG2 = 3 |


#### 5.6.3 애플리케이션 프로토콜

데이터 필드는 **CMD(1 Byte) + VALUE(1 Byte)** 의 2바이트 구조로 정의된다.

| **CMD** | **CMD 코드** | **VALUE** | **동작** |
| :---: | :---: | :---: | :--- |
| TRACE_CTRL | `0x10` | `0x01` / `0x00` | 광원 추적 ON / OFF |
| SOLAR_CTRL | `0x11` | `0x01` / `0x00` | 태양광 충전 ON / OFF |
| FORCE_CTRL | `0x12` | `0x01` / `0x02` | 강제 정지 / 재초기화 |
| DRIVE_CTRL | `0x13` | `0x01` / `0x00` | 자율주행 ON / OFF |

### 5.7 광원 추적 시스템 (Solar Tracking System)

#### 5.7.1 센서 배치 및 회로 구성

광원 추적 시스템은 **4채널 CDS 조도 센서** 의 상대 비교 방식으로 태양의 방향을 추정한다. 센서는 패널 상·하·좌·우 4개 위치에 배치되며, 각 센서의 조도 차이가 서보 모터의 회전 방향을 결정한다.

| **채널** | **ADC CH** | **MCU 핀** | **위치** |
| :---: | :---: | :---: | :---: |
| S1 | CH4 | PA4 | 우상 (Right-Top) |
| S2 | CH8 | PB0 | 우하 (Right-Bottom) |
| S3 | CH10 | PC0 | 좌상 (Left-Top) |
| S4 | CH11 | PC1 | 좌하 (Left-Bottom) |

각 센서는 **10 kΩ 고정 저항과 CDS 센서의 풀업 전압 분배 회로** 로 구성되며, 조도에 따라 변하는 CDS 저항값이 출력 전압으로 변환된다.

$$V_{out} = V_{cc} \times \frac{R_{fixed}}{R_{fixed} + R_{CDS}}$$

신호 안정성 확보를 위해 **저역통과 필터(LPF, $f_c \approx 15.9\ Hz$)** 와 **LM358N OP-AMP 전압 버퍼($A_v = 1$)** 를 적용하여 고주파 노이즈 제거 및 임피던스 매칭을 수행한다. 이후 **ADC1 12비트 연속 변환(DMA)** 으로 4채널을 순차 샘플링하여 `adcValue[4]` 배열에 저장한다.

$$V_{ADC} = \frac{ADC_{raw}}{4096} \times V_{ref}$$


#### 5.7.2 태양 방향 추정 (error_x / error_y)

필터링된 4채널 값으로 좌우(`error_x`) 와 상하(`error_y`) 오차를 각각 계산한다.

$$error\_x = (S3 + S4) - (S1 + S2) \quad \text{(좌측 합 - 우측 합)}$$

$$error\_y = (S1 + S3) - (S2 + S4) \quad \text{(상측 합 - 하측 합)}$$

| **오차 부호** | **의미** | **서보 동작** |
| :---: | :---: | :---: |
| $error\_x > +800$ | 좌측이 더 밝음 | Pan 감소 (← 좌 회전) |
| $error\_x < -800$ | 우측이 더 밝음 | Pan 증가 (→ 우 회전) |
| $error\_y > +800$ | 상측이 더 밝음 | Tilt 증가 (↑ 상 회전) |
| $error\_y < -800$ | 하측이 더 밝음 | Tilt 감소 (↓ 하 회전) |

임계값 **THRESHOLD = 800** 은 **Dead Zone** 로 작용하여 미세 진동에 의한 불필요한 서보 진동(Jitter)을 방지한다.


---

## 🏁 6. Final Product & Demonstration (완성품 및 시연)



### 6.1Final Product (완성품) 및 제작 과정

| **리모컨 제작 (전면 & 후면부)** | **감지 모듈 PCB 제작 (상부 & 하부 사진)** |
| :---: | :---: |
| <img src="images/1.jpg" width="150">&nbsp;<img src="images/2.jpg" width="150"> | <img src="images/3.jpg" width="150">&nbsp;<img src="images/4.jpg" width="150"> | 
| **광원 추적 시스템** | **Buck Converter 기반 배터리 충전 시스템** |
| **(1) 구동 파트 제작 (상부 & 하부 사진)** | **(1) 구동 파트 제작 (좌측 & 우측 사진)** |
| <img src="images/5.jpg" width="150">&nbsp;<img src="images/6.jpg" width="150"> | <img src="images/7.jpg" width="150">&nbsp;<img src="images/8.jpg" width="150"> |
| **(2) 광원 추적 모듈 제작 (회로 기판 & 회전 장치 사진)** | **(2) 배터리 충전 모듈 제작** |
| <img src="images/9.jpg" width="150">&nbsp;<img src="images/10.jpg" width="150"> | <img src="images/11.jpg" width="150"> |
| **(3) 동작 구현 (좌측 & 우측 사진)** | **(3) 동작 구현 (좌측 & 우측 사진)** |
| <img src="images/12.jpg" width="150">&nbsp;<img src="images/13.jpg" width="150"> | <img src="images/14.jpg" width="150">&nbsp;<img src="images/15.jpg" width="150"> |
| **(4) 최종 시스템 구현 (전면 & 후면 사진)** | **(4) 최종 시스템 구현 (좌측 & 우측 사진)** |
| <img src="images/16.jpg" width="150">&nbsp;<img src="images/17.jpg" width="150"> | <img src="images/18.jpg" width="150">&nbsp;<img src="images/19.jpg" width="150"> |


### 6.2 Demonstration (시연 영상)


<a href="https://www.youtube.com/playlist?list=PLfvFIjdRKo629Y1z1ofcWMX5VUU1T6zAE" target="_blank">
  <img src="images/youtube.jpg" alt="Watch Demo Video" width="300" />
</a>


**Demonstration Check List:**

| **항목** | **검증 내용** |
| :---: | :--- |
| **01. Temp Stress Test** | 고온 환경에서의 온도 센서 응답 및 속도 제한 동작 검증 |
| **02. Gas Stress Test** | MQ-135 센서 baseline 대비 가스 농도 변화 감지 검증 |
| **03. Electrical Stress Test** | INA219 기반 과전류 / 과전압 / 저전압 보호 동작 검증 |
| **04. BMS Stress Test** | 다중 WARNING 발생 시 단계적 속도 제한 및 latch 구조 검증 |
| **05. Solar Charge System Simulation** | CC → MPPT → CV 모드 전환 및 충전 효율 검증 |
| **06. Solar Trace Simulation** | 4채널 CDS 센서 기반 Pan/Tilt 추적 동작 검증 |

---

## 7. Troubleshooting (문제 해결 기록)
  


### 7.1 2차 모델의 한계와 4차 소신호 모델 확장

🔍 **Issue (문제 상황)**

- 실제 측정 응답과 비교 시 전류 응답에서 목표값 대비 **약 10~20% 수준의 편차**와 **정착시간 차이**가 발생하여 PI 제어기 K값의 신뢰도가 낮아짐

❓ **Analysis (원인 분석)**

- 초기 2차 모델은 Buck 출력단의 인덕터 전류와 커패시터 전압만 고려하여 입력단 동특성을 반영하지 못함
- 태양전지는 광량과 동작점에 따라 전압·전류가 변하는 **비선형 전원**으로, Buck 입력단 전압 변화가 제어 응답에 직접적인 영향을 미침
- 실제 시스템에서는 INA219를 통해 입력단과 출력단 전압·전류를 모두 계측하므로, 입력 전력 및 전압 변동이 포함된 동특성 모델이 필요함
- 이로 인해 2차 모델 기반 전달함수와 실제 시스템 응답 간 오차가 발생

❗ **Action (해결 방법)**

- 상태변수를 $x = [v_{pv}, v_{in}, i_L, v_o]^T$ 로 정의하여 입력단과 출력단을 모두 포함한 **4차 소신호 모델** 구성
- SW ON / SW OFF 상태별 미분방정식 도출 후 듀티비 $d$ 를 이용한 **평균화 비선형 모델** 유도
- 정상상태 동작점 주변 선형화를 통해 상태공간 행렬 $A$, $B$ 도출 및 PI 제어기 설계

✅ **Result (결과)**

- Bode Plot 분석을 통해 전류 루프 **PM 63.1°**, 전압 루프 **PM 168.6°** 의 안정적인 위상 여유 확보
- 비선형 시뮬레이션 및 실측에서 **약 0.8 A 기준 안정적 수렴** 및 MPPT–CC–CV 모드 전환 정상 동작 확인
- 2차 모델 대비 제어기 설계 정확도가 향상되어 실제 시스템과의 응답 오차를 효과적으로 감소시킴


---

### 7.2 I2C 노이즈 간섭

🔍 **Issue (문제 상황)**

- INA219를 통한 I2C 통신이 불안정하여 센서 초기화가 실행되지 않는 오류 발생

❓ **Analysis (원인 분석)**

- INA219는 I2C 통신을 사용하며, 이는 외부 전자기 노이즈에 민감한 특성을 가짐
- Buck Converter 인덕터가 스위칭 동작 시 자기 노이즈를 발생시키며, INA219의 I2C 신호선과 근접 배치되어 노이즈 간섭이 유입된 것으로 판단됨

❗ **Action (해결 방법)**

- INA219와 인덕터 간 물리적 이격 거리를 확보하여 자기 노이즈 간섭 경로를 차단

✅ **Result (결과)**

- INA219 센서 초기화 오류가 해소되어 정상적인 I2C 통신 및 센서 계측이 가능해짐

---

### 7.3 태양광 충전 모드 전환 문제

🔍 **Issue (문제 상황)**

- 배터리 전압이 낮음에도 불구하고 CC 모드로 진입하지 않고 MPPT 또는 CV 상태로 동작하는 문제 발생

❓ **Analysis (원인 분석)**

- MPPT 알고리즘이 전류를 감소시키는 과정에서 **i_mppt_a(MPPT 전류 한계값)**가 **음수**로 내려갈 수 있었음
- 충전 전류 결정 로직이 `i_ref = min(Icc, Icv, Imppt)`로 구성되어, Imppt가 **음수**가 되면 `min()` 결과가 **0A**로 고정됨
- 이로 인해 충전이 중단되고 CC 모드로의 전환이 이루어지지 않음

❗ **Action (해결 방법)**

- `i_mppt_a` 값을 다음 두 가지 형태만 허용하도록 정규화 함수를 도입
- 0A (**충전 중지**) 또는 **0.05A ~ 0.90A** 범위 (**실제 제어 가능 영역**) 로 강제 제한

✅ **Result (결과)**

- MPPT 전류값의 음수 하락이 방지되어 CC·CV·MPPT 모드 전환이 정상적으로 동작함

---

### 7.4 기판 제작 중 전력 손실 문제

🔍 **Issue (문제 상황)**

- 충전 기판 제작 시 전압 강하 및 전류 손실로 인해 충전 성능이 저하되는 문제 발생

❓ **Analysis (원인 분석)**

- 태양광 충전 회로의 전력 공급 라인에 단면적이 작은 점퍼선을 사용하여, 충전 전류 흐름 시 도선 저항에 의한 전력 손실이 발생한 것으로 확인됨

❗ **Action (해결 방법)**

- 전력 공급 라인을 단면적이 충분한 단선(Solid Wire)으로 교체하여 충전 경로의 전기 저항을 저감

✅ **Result (결과)**

- 전압 강하 및 전류 손실이 제거되어 충전 성능이 정상 수준으로 회복됨


---

### 7.5 차량 무게로 인한 회전 문제

🔍 **Issue (문제 상황)**

- 초음파 센서 기반 자율주행 동작 시 차량이 정상적으로 회전하지 못하는 문제 발생

❓ **Analysis (원인 분석)**

- 초기 자율주행 알고리즘은 차체 무게가 상대적으로 가벼운 상태를 기준으로 설계됨
- 이후 태양광 추적 시스템, 배터리 등의 부품이 차량에 탑재되면서 총 중량이 증가함
- 증가된 무게로 인해 기존 PWM 기반 회전 동작의 회전력이 부족하여 정상적인 방향 전환이 불가함

❗ **Action (해결 방법)**

- 회전력 부족 문제를 해결하기 위해 **피벗 턴(Pivot Turn)** 방식을 도입
- 회전 방향의 바퀴를 정지시키고 반대 방향 바퀴를 역방향으로 구동하여 제자리 회전 구현

✅ **Result (결과)**

- 차량 중량 증가에도 불구하고 안정적인 방향 전환이 가능해져 자율주행이 정상 동작함

---

### 7.6 태양광 추적시스템 오동작

🔍 **Issue (문제 상황)**

- 태양광 추적 시스템이 광원 방향으로 추적하지 않고 반대 방향으로 이동하는 오동작 발생
- 브레드보드 테스트 단계에서는 정상 동작하였으나 만능 기판 제작 후 오동작 확인됨

❓ **Analysis (원인 분석)**

- 태양광 추적 시스템은 조도 센서 4개를 평면 좌표계에 배치하여 각 센서의 조도 차이를 비교하고, 그 증감에 따라 추적 방향을 결정하는 구조임
- 만능 기판에 센서 모듈을 납땜·장착하는 과정에서 **90도 회전된 상태로 고정**되어, 소프트웨어에 정의된 좌표 매핑과 실제 물리적 배치가 불일치하게 됨

❗ **Action (해결 방법)**

- 90도 회전 후 실제 물리 배치를 기준으로 센서 좌표 매핑을 재정의하여 소프트웨어에 반영
- **left = S1 + S3** → **left = S3 + S4**
- **right = S2 + S4** → **right = S1 + S2**
- **top = S1 + S2** → **top = S1 + S3**
- **bottom = S3 + S4** → **bottom = S2 + S4**

✅ **Result (결과)**

- 좌표 재매핑 이후 태양광 추적 시스템이 광원 방향으로 정확하게 추적 동작함

---

## 📦 8. Bill of Materials (주요 부품 리스트)

> 구매처: 디바이스마트 / 상세 BOM 은 `공모전파일/` 디렉토리 참조

### 8.1 태양광 충전 시스템

| 분류 | 부품명 | 비고 |
| :---: | :---: | :---: |
| 태양전지 | D165x165-3 (6 V 4.5 W) | - |
| 전력 소자 | IRF9540N / 1N5822 / 2N2222A | P-MOSFET / Diode / BJT |
| 인덕터 | RING COIL 11파이 (220 µH) | - |
| 캐패시터 | E/C 16 V 220 µF / 50 V 10 µF / 16 V 470 µF | - |
| 전류 센서 | INA219 (SZH-SSBH-075) | I2C |
| 배터리 | UB848 (18650 3.7 V 2600 mAh) | - |

### 8.2 광원 추적 시스템

| 분류 | 부품명 | 비고 |
| :---: | :---: | :---: |
| 서보 모터 | TowerPro MG996R | Pan / Tilt |
| 조도 센서 | CdS Cell GL5549 | 4 채널 |
| 전원 | SRS-155 (3 V 150 mA) | - |
| OP-AMP | LM358N | 전압 버퍼 |
| Buck Module | LM2596HV | - |

### 8.3 배터리 안전 관리 시스템

| 분류 | 부품명 | 비고 |
| :---: | :---: | :---: |
| 온도 센서 | NTC-10KGJG | 10 kΩ Thermistor |
| 가스 센서 | MQ-135 (SZH-SSBH-038) | - |
| 전류 센서 | INA219 (SZH-SSBH-075) | I2C |
| OP-AMP | LM358N | LPF 버퍼 |

### 8.4 차량 주행 제어 시스템

| 분류 | 부품명 | 비고 |
| :---: | :---: | :---: |
| MCU | NUCLEO-F411RE | Master Board |
| 모터 드라이버 | L298N (SZH-EK001) | 2 A |
| 초음파 센서 | HC-SR04 (SZH-USBC-004) | 3 채널 |
| Bluetooth | HC-05 / HC-06 | - |
| CAN 컨트롤러 | MCP2515 + TJA1050 | SPI |

### 8.5 무선 조종부

| 분류 | 부품명 | 비고 |
| :---: | :---: | :---: |
| MCU | STM32F411 BlackPill | Adafruit ada-4877 |
| 조이스틱 | PS2 듀얼 로커 모듈 (ELB070682) | - |
| Bluetooth | HC-05 (SZH-EK069) | - |

