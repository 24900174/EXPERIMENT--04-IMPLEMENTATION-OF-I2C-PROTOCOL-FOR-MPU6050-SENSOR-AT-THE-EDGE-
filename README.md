 # EXPERIMENT-03-INTERFACING-DIGITAL-SENSOR-WITH-EDGE-DEVELOPMENT-BOARD-

---

### NAME:R.Prabanjan 
### DEPARTMENT:AIDS
### ROLL NO:212224230198 
### DATE OF EXPERIMENT:24/03/2025

---

## **AIM:**  
To interface an **MPU6050 6-Axis Accelerometer & Gyroscope Sensor** with the **Raspberry Pi Pico** and display the sensor readings using MicroPython.


---

## **APPARATUS REQUIRED:**  
1. Raspberry Pi Pico  
2. MPU6050 6-Axis Accelerometer & Gyroscope Sensor  
3. Jumper Wires  
4. Breadboard  
5. USB Cable  
6. Computer with Thonny IDE  

---

## **THEORY:**  
### **About Raspberry Pi Pico:**  
Raspberry Pi Pico is a microcontroller board based on the **RP2040 chip**. It features:  
- Dual-core ARM Cortex-M0+ processor  
- 26 multi-function GPIO pins  
- Supports **MicroPython** and **C/C++**  
- Interfaces like **I2C, SPI, UART, and PWM**  
- Low power consumption, ideal for **IoT applications**  

### **About MPU6050 Sensor:**  
The **MPU6050** is a **6-Axis Inertial Measurement Unit (IMU)** that includes:  
- **3-axis accelerometer** and **3-axis gyroscope**  
- **I2C communication protocol** for easy interfacing  
- **Operating Voltage:** 3.3V – 5V  
- **Accelerometer Range:** ±2g, ±4g, ±8g, ±16g  
- **Gyroscope Range:** ±250°/s, ±500°/s, ±1000°/s, ±2000°/s  

The **accelerometer** measures linear acceleration in **X, Y, Z axes**, while the **gyroscope** measures rotational velocity. The sensor communicates with the Raspberry Pi Pico via **I2C protocol**.

---

## **WORKING PRINCIPLE:**  
1. The **MPU6050 sensor** is connected to the **Raspberry Pi Pico** using the **I2C communication protocol**.  
2. The **Pico reads acceleration and gyroscope values** from the sensor registers.  
3. The data is **processed and displayed on the serial monitor**.  
4. The readings can be used for **motion tracking, tilt sensing, or gesture recognition**.

---

## **CIRCUIT DIAGRAM:** 
![Screenshot 2025-03-10 115048](https://github.com/user-attachments/assets/ee80b2e1-4ade-4d02-a68d-6400811d689a)
### **Connections:**  

| MPU6050 Pin | Raspberry Pi Pico Pin |
|------------|----------------------|
| VCC | 3.3V |
| GND | GND |
| SDA | GP20 |
| SCL | GP21 |

---

## **PROGRAM (MicroPython)**  
```python
from machine import Pin, I2C
import utime

# MPU6050 I2C Address
MPU6050_ADDR = 0x68

# MPU6050 Registers
PWR_MGMT_1 = 0x6B
ACCEL_XOUT_H = 0x3B
GYRO_XOUT_H = 0x43

# Initialize I2C on Raspberry Pi Pico (GPIO 0 and GPIO 1)
i2c = I2C(0, scl=Pin(1), sda=Pin(0), freq=400000)  # I2C0 with 400kHz frequency

def mpu6050_init():
    """Initialize MPU6050 by waking it up from sleep mode."""
    i2c.writeto_mem(MPU6050_ADDR, PWR_MGMT_1, b'\x00')  # Wake up MPU6050

def read_raw_data(reg):
    """Read 2 bytes of raw data from the given register."""
    data = i2c.readfrom_mem(MPU6050_ADDR, reg, 2)  # Read 2 bytes
    value = (data[0] << 8) | data[1]  # Combine high and low bytes
    if value > 32767:  # Convert to signed 16-bit integer
        value -= 65536
    return value

# Initialize MPU6050
mpu6050_init()

# Read and display Accelerometer and Gyroscope data
while True:
    ax = read_raw_data(ACCEL_XOUT_H)/16384.0
    ay = read_raw_data(ACCEL_XOUT_H + 2)/16384.0
    az = read_raw_data(ACCEL_XOUT_H + 4)/16384.0

    gx = read_raw_data(GYRO_XOUT_H)/131.0
    gy = read_raw_data(GYRO_XOUT_H + 2)/131.0
    gz = read_raw_data(GYRO_XOUT_H + 4)/131.0

    print(f"Accel: X={ax}, Y={ay}, Z={az} | Gyro: X={gx}, Y={gy}, Z={gz}")

    utime.sleep(10)  # Wait for 1 second before reading again
```

---

## **OUTPUT:**  
When the above program is executed, the output on the serial monitor will display real-time acceleration and gyroscope values, such as:
![IMG-20250324-WA0007](https://github.com/user-attachments/assets/dd6fec64-43da-4c79-8270-c73ba64adf5d)
![IMG-20250324-WA0006](https://github.com/user-attachments/assets/72a5d4a6-b7f5-4180-be33-27de110d43fc)
![IMG-20250324-WA0009](https://github.com/user-attachments/assets/662bca25-1b12-4136-bcee-138ca2a1c83d)
![IMG-20250324-WA0008](https://github.com/user-attachments/assets/7c42bb52-fc81-4a31-92fe-345188182739)

```

Accel: X=0.02g, Y=-0.01g, Z=1.00g | Gyro: X=0.05°/s, Y=-0.02°/s, Z=0.01°/s
Accel: X=0.03g, Y=-0.02g, Z=1.01g | Gyro: X=0.06°/s, Y=-0.03°/s, Z=0.02°/s
...
```
---

## **RESULT:**  
The **MPU6050 sensor** was successfully interfaced with the **Raspberry Pi Pico**, and real-time **acceleration and gyroscope data** were read and displayed. The sensor values can be used for **motion tracking, tilt detection, and gesture control applications**.

---

