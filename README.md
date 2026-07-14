# ASTRA FLIGHT CONTROLLER
The Astra Flight Controller is a custom-designed flight control board developed for multirotor unmanned aerial vehicles (UAVs). It is built around the STM32F411CEU6 microcontroller and the ICM-42688-P six-axis Inertial Measurement Unit (IMU), providing a compact, high-performance platform capable of running modern flight control firmware such as Betaflight, INAV, or customdeveloped firmware
1. **Main Features**
* STM32F411CEU6 ARM Cortex-M4 Microcontroller  
* ICM-42688-P 6-axis IMU  
* MP2359DJ High-Efficiency 5 V Buck Converter  
* AP2112K 3.3 V Low-Dropout Voltage Regulator  
* USB Type-C Interface  
* SWD Programming Header  
* XT30 Battery Input  
* Active Buzzer Driver Circuit  
* Status LED  
* External UART Expansion  
* Dedicated SPI Interface for the IMU  
* Compact PCB designed within a 100 mm × 100 mm manufacturing limit
  2. **Hardware Architecture**  
* Power Management  
* Processing Unit  
* Sensor System  
* Communication Interfaces  
* User Interface  
* Programming & Debugging  
* External Connections  
 3. **Hardware Modules Power Management**
Responsible for converting battery voltage into stable regulated voltages used by the board.
Major Components:
* XT30 Battery Connector  
* Resettable Fuse  
* MP2359DJ Buck Converter  
* AP2112K 3.3V Regulator Filtering Capacitors Outputs:  
 +5V Rail  
 +3.3V Rail  
4. **Processing Module**
The processing module is the "brain" of the flight controller. Main Component: STM32F411CEU6.
Responsibilities:  
* Reads sensor data  
* Executes flight control algorithms  
* Controls motors  
* Handles USB communication  
* Processes receiver input  
* Generates PWM/DShot signals    
* Controls LEDs and buzzer  
 5. **Sensor Module** 
The sensor subsystem provides real-time motion data.  
Main Sensor: ICM-42688-P.  
 
Measurements: 3-axis Accelerometer, 3-axis Gyroscope. Communication: SPI Interface.  
The IMU continuously measures angular velocity and acceleration  
6. **Power-Up Sequence**
When battery power is applied, the following sequence occurs:  
Step 1: Battery voltage enters through the XT30 connector.  
Step 2: The resettable fuse protects the board from excessive current.   
Step 3: The MP2359DJ converts battery voltage into a regulated +5V supply.   
Step 4: The AP2112K converts +5V into +3.3V.   
Step 5: The STM32F411CEU6 receives stable power.   
Step 6: The crystal oscillator starts generating the system clock.  
Step 7: The microcontroller boots from Flash memory.   
Step 8: The SPI interface initializes the ICM-42688-P.     
Step 9: Sensors begin transmitting motion data.     
Step 10: The flight controller enters normal operating mode.  
------------------------------------------------------------------------------------------------------------------------------------------


**Clock Generation**
The STM32F411CEU6 uses an external crystal oscillator for accurate timing. The external crystal provides stable CPU clock, accurate USB timing, reliable SPI communication, and consistent sensor sampling. Proper placement of the crystal and its load capacitors is essential for reliable operation.  

**power system**
Why Two Regulators?  
Using a single linear regulator directly from the battery would result in excessive heat generation, especially at higher input voltages. Instead, the Astra Flight Controller uses a two-stage regulation    
approach:  
● MP2359DJ Buck Converter — efficiently reduces battery voltage to 5 V.  
● AP2112K LDO — produces a clean and stable 3.3 V output with minimal electrical  
noise. This combination provides both efficiency and stable operation.  

**STM32F411CEU6 Microcontroller**
The STM32F411CEU6 is the primary processing unit of the Astra Flight Controller. It executes the flight control firmware, processes sensor data, generates motor outputs, and manages all communication interfaces.
Role-   
The STM32 reads IMU data through SPI, executes flight control algorithms, generates ESC outputs, monitors battery voltage, controls the LED and buzzer, manages USB communication, and supports firmware programming through SWD.  

## ICM-42688-P Inertial Measurement Unit (IMU)  
 *Introduction*  
The ICM-42688-P is the primary motion sensing device used in the Astra Flight Controller. It integrates a 3-axis accelerometer and a 3-axis gyroscope in a compact LGA package. The sensor continuously measures aircraft motion and sends this data to the STM32F411CEU6 through the SPI interface for attitude estimation and flight stabilization.    
**Why the ICM-42688-P was Selected?**  
The sensor was selected because of its excellent gyroscope noise performance, high sampling rate, low latency, low power consumption, compact size, and broad support in modern flight-control firmware such as Betaflight and INAV.  
Main Specifications:-  
* Device- ICM-42688-P  
* Sensor Type- 6-Axis IMU  
* Accelerometer- 3-Axis MEMS  
* Gyroscope- 3-Axis MEMS  
* Operating Voltage- 1.71–3.6 V  
* Communication- SPI / I²C  
* Max SPI Clock Up to 24 MHz  
* Operating Temp −40°C to +85°C    
**Working Principle**  
The accelerometer measures linear acceleration along the X, Y, and Z axes, while the gyroscope  
measures angular velocity about the same axes. Together they provide six degrees of freedom (6-DoF), allowing the firmware to estimate   the aircraft's orientation using sensor-fusion algorithms.  
**Hardware Integration**  
The IMU operates from the regulated 3.3 V rail. VDD and VDDIO are connected to 3.3 V, while the GND pins connect directly to the ground plane. Local decoupling capacitors are placed close to the power pins to reduce supply noise and improve stability.    
**SPI Communication**  
Communication with the STM32F411CEU6 takes place over SPI using SCK, MOSI, MISO, and CS. SPI was selected instead of I²C because it offers higher bandwidth, lower latency, and improved reliability in electrically noisy environments.  
**Interrupt Output**
The interrupt pin notifies the STM32 whenever new sensor data is available, reducing processor workload and improving the timing of the flight-control loop.  

The IMU continuously measures aircraft motion, sends accelerometer and gyroscope data to the STM32 through SPI, and the firmware processes this information to estimate attitude and generate motor control outputs.  

## USB Type-C Interface

*Introduction*  
The USB Type-C interface serves as one of the primary communication and power interfaces of the Astra Flight Controller. It provides firmware flashing, debugging, configuration, diagnostics and  
development power.  
USB Type-C offers reversible insertion, improved durability, better current capability and long-term compatibility. The design uses the USB 2.0 Full-Speed interface connected directly to the STM32F411CEU6.   

**Purpose**
Firmware upload, configuration using Betaflight/INAV/STM32CubeProgrammer, debugging through serial communication, and powering the board during development without a LiPo battery  
**Advantages**  
Reversible connector, stronger mechanical retention, higher current capability and modern industrystandard compatibility.  
**USB Connector**
| Pin | Function |
| VBUS | +5V |
| GND | Ground |
| D+ | USB Data Positive |
| D- | USB Data Negative |
| CC1 | Configuration Channel 1 |
| CC2 | Configuration Channel 2 |
| Shield | Connector Shield |

**Data Communication**
USB Full-Speed uses the D+ and D− differential pair. Route them together with matched lengths, minimal vias, constant spacing, no sharp bends and a continuous ground plane. The STM32F411CEU6 supports USB 2.0 Full-Speed (12 Mbps), sufficient for firmware updates and configuration  
##  XT30 Battery Connector
The XT30 connector is the primary battery input. Battery voltage enters the board through the XT30 connector, passes through the resettable fuse and protection circuitry, and then feeds the MP2359DJ buck converter. Wide copper traces are used to carry battery current and minimise voltage drop.  
## ESC Connections
Motor control signals generated by the STM32 are routed to the ESC outputs. These connections carry logiclevel control signals only; motor power is supplied separately by the battery through the ESCs.   
#  Indicators and User Interface
**Overview**  
The Astra Flight Controller includes a status LED, a TL3342 push button and an active buzzer. These devices provide visual, audible and manual interaction during development, testing and flight operation.  
**Status LED**
The STM32 controls the LED to indicate power, boot status, firmware activity, calibration, arming state and error conditions.  
**Push Button**
The TL3342 tactile switch can be used for boot mode selection, firmware recovery, calibration and user-defined functions through a GPIO input.  
**Active Buzzer**
The active buzzer provides audible feedback including startup, arming, low-battery and fault indications. It requires only an on/off control signal from the STM32.  
## Component Placement Strategy
1. Microcontroller Placement  
The STM32F411CEU6 is positioned near the center of the PCB. This location minimizes the average distance between the MCU and all connected peripherals, resulting in shorter SPI traces, shorter USB connections, reduced propagation delay, easier routing, and improved signal integrity. The exposed thermal pad beneath the MCU is connected directly to the ground plane for both thermal dissipation and electrical grounding.  
2. IMU Placement  
The ICM-42688-P is the most sensitive component on the board. To maximize Measurement accuracy, it is placed close to the center of the PCB, kept away from the MP2359DJ buck converter and inductor, and highcurrent traces are not routed beneath the sensor. A continuous ground plane is maintained underneath the device, and the sensor orientation matches the coordinate system  
expected by the firmware.  
Proper placement reduces electrical interference and vibration-induced measurement errors  
**Power Section Placement**
The power conversion circuitry is grouped together near the battery input. This includes:  
● XT30 connector.  
● Resettable fuse.  
● Protection diode.  
● MP2359DJ buck converter.  
● 4.7 µH Bourns SRP5030TA-4R7M inductor.  
● Input and output capacitors.  
● AP2112K voltage regulator.  
## Power Supply Design
The Astra Flight Controller is designed to operate directly from a 2S to 6S Lithium Polymer (LiPo) battery, meaning the input voltage can vary from approximately 7.4 V to 25.2 V. To achieve stable regulation, the Astra Flight Controller uses a two-stage power architecture: the MP2359DJ synchronous buck converter generates a regulated 5 V supply from the battery, and the AP2112K Low Dropout (LDO) regulator generates a clean 3.3 V supply from the 5 V raiL  

 Power Flow
2S–6S LiPo Battery  
▼  
XT30 Battery Connector  
▼  
Resettable Fuse (F1)  
▼  
TVS / Reverse Protection  
▼  
MP2359DJ Buck Converter — +5 V Rail  
▼  
AP2112K LDO Regulator — +3.3 V Rail  
▼   
STM32F411 
▼  
ICM-42688  
##  Resettable Fuse (F1)
Immediately after the XT30 connector, the input passes through a resettable polyfuse (PTC), which protects the PCB from excessive current caused by short circuits, incorrect wiring, component failures, or assembly mistakes. Unlike a traditional fuse, the polyfuse automatically resets after the fault is removed and the device cools down.  

## MP2359DJ Buck Converter
1. Why a Buck Converter?
A linear regulator would dissipate the voltage difference between the battery and the output as heat. Converting 25.2 V from a fully charged 6S battery directly to 5 V using a linear regulator would result in excessive power loss and unacceptable temperatures. A switching buck converter solves this problem by rapidly switching the input voltage through an inductor and filtering the output,
achieving efficiencies typically above 85–90%.  
2. MP2359DJ Features  
* Wide input voltage range.  
* High conversion efficiency.  
* Integrated MOSFETs.  
* Compact package.  
* High switching frequency.  
* Internal protection features.  
* Minimal external components.  
3. Operating Principle  
The MP2359DJ is a synchronous step-down (buck) switching regulator. Its operation consists of four basic stages: the internal MOSFET switches ON allowing current to flow into the inductor; energy is stored in the magnetic field of the inductor; the MOSFET switches OFF; and the inductor releases stored energy through the output filter, maintaining a continuous output voltage. This cycle repeats
hundreds of thousands of times per second.
External Components-- 
● Input capacitor.
● Output capacitor.
● Bootstrap capacitor.
● Schottky protection diode (where applicable).
● Feedback resistor network.
● 4.7 µH power inductor



 # Manufacturing 
The PCB is designed to be compatible with standard manufacturing services such as JLCPCB while
remaining within the 100 mm × 100 mm board size 


*Schematic*
<img width="1754" height="1241" alt="Flight controler_page-0001 (1)" src="https://github.com/user-attachments/assets/a6c6a997-dd35-4039-b541-97a28f8f33ff" />
*PCB tracing* 
<img width="1366" height="727" alt="Screenshot (255)" src="https://github.com/user-attachments/assets/5f22b8b1-5ed2-40b2-9cae-7695a9d245d9" />
*3D view*
<img width="1266" height="727" alt="Screenshot (254)" src="https://github.com/user-attachments/assets/bc659ab7-eb4f-4201-91d0-d8095827e179" />
 
 * Note-- the firmware needs to be programed according to the use, hence cannot be pre-programed
   

