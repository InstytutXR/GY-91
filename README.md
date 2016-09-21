GY-91 (MPU-9250 + BMP280)
========

This is a fork of Kris Winer's MPU-9250 to make use of the BMP280 on board on the GY-91. My modifications are simplistic at best, he did ALL the hard work! Thank you Kris.

Arduino sketch for GY-91 module containing MPU-9250 and BMP280 10 DoF sensor with AHRS sensor fusion

Most modern and correct version is:  	MPU9250_MS5637_AHRS_t3.ino, all require quaternionFilters.ino in the IDE folder also to use the Madgwick and/or Mahony sensor fusion algorithms.

Demonstrate MPU-9250 and BMP280 basic functionality including parameterizing the register addresses, initializing the sensor, getting properly scaled accelerometer, gyroscope, magnetometer, altitude, pressure and temperature data out, calibration and self-test of sensors. Addition of 10 DoF sensor fusion using open source Madgwick and Mahony filter algorithms. Sketch runs on esp8266-12e NodeMCU board.

A discussion of the use and limitations of this sensor and sensor fusion in general is found ![here.]
(https://github.com/kriswiner/MPU-6050/wiki/Affordable-9-DoF-Sensor-Fusion)

To use this with the esp8266 install the arduino IDE core from here: https://github.com/esp8266/Arduino/

I have also added a program to allow sensor fusion using the MPU-9250 9-axis motion sensor with the STM32F401 Nucleo board using the mbed compiler. The STM32F401 achieves a sensor fusion filter update rate using the Madgwick MARG fusion filter of 4800 Hz running the M4 Cortex ARM processor at 84 MHz; compare to the sensor fusion update rate of 2120 Hz achieved using the same filter with the Teensy 3.1 running its M4 Cortex ARM processor at 96 MHz.

One reason for this difference is the single-precision floating point engine embedded in the STM32F401 core. While both ARM processors achieve impressive rates of filtering, really more than necessary for most applications, the factor of two difference translates into much lower power consumption for the same sensor fusion performance. If adequate sensor fusion filtering, say, 1000 Hz, can be achieved at much lower processor clock speed, then over all power consumption will be reduced. This really matters for wearable and other portable motion sensing and control applications.

It can be simply modified to work with the corresponding micro shield as well. It uses SDA/SCL on pins 17/16, respectively, and it uses the Teensy 3.1-specific Wire library i2c_t3.h. The MS5637 is a simple but high resolution pressure sensor, which can be used in its high resolution mode with power consumption of 20 microAmp, or in a lower resolution mode with power consumption of only 1 microAmp. The choice will depend on the application. The sketch calculates and outputs temperature in degrees Centigrade, pressure in millibar, and altitude in feet. In high resolution mode, the pressure is accurate to within 10 Pa or 0.1 millibar, and the height discrimination is about 13 cm. This is much better performance than achievable from the venerable MPL3115A2 and the MS5637 is in a very small package perfect for the small micro and mini add-on Teensy 3.1 shields.

For a discussion of the relative merits of modern board-mounted pressure sensors, see [here](https://github.com/kriswiner/MPU-9250/wiki/Small-pressure-sensors).

I added sketches for the various new Mini add-on shields for Teensy 3.1 with the MPU9250 9-axis motion sensor and either the MPL3115A2 or the newer LPS25H pressure sensor/altimeter. Now there are three flavors of 10 DoF Mini add-on boards specially designed for the Teensy 3.1 with state-of-the-art 20-bit (MPL3115A2) and 24-bit (MS5637 and LPS25H) altimeters. The LPS25H has a 32-byte FIFO and sophisticated hardware filtering which allows very low power operation while maintaining 0.01 millibar resolution. This is really quite a feat; this sensor deserves serious consideration for any airborne application you might have in mind.
