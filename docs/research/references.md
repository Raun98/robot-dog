# Research references

Official and community links used during Phase 0. These are citations, not a license to copy CAD or binaries.

## Raspberry Pi and Hailo

- [Raspberry Pi AI HAT+](https://www.raspberrypi.com/documentation/accessories/ai-hat-plus.html)
- [Set up AI Kit / Hailo on Pi 5](https://www.raspberrypi.com/news/how-to-set-up-the-raspberry-pi-ai-kit-with-raspberry-pi-5/)
- [hailo-ai/hailo-rpi5-examples — Pi 5 install](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/install-raspberry-pi5.md)
- [Pi camera installation](https://www.raspberrypi.com/documentation/accessories/camera.html)

## Quadruped references (cite, do not clone)

- Mini Pupper (MangDang) — small 12-DOF educational quadruped
- SpotMicro — open-ish hobby Spot-like kinematics
- Stanford Pupper / Pupper v2
- CHAMP — ROS quadruped controllers

## Actuators

- MG995 analog hobby servo — v1 walking actuators (12 owned)
- Waveshare ST3215 = Feetech STS3215 TTL bus servos — deferred; same family; India ~₹1,800–2,900 each (Rees52 / Hubtronics / Tomson)
- DIY 3D-printed joint — **not v1**; ₹1,200/joint cap is micro-only: [diy-actuators.md](diy-actuators.md)
- OpenTorque / OpenQDD / printed cycloidal + 5008 — cite as QDD class; too expensive vs cap
- mjbots [qdd100](https://mjbots.com/products/qdd100-beta-3) (~USD 879) — too expensive
- [ODrive Micro](https://shop.odriverobotics.com/products/odrive-micro) (USD 89/motor) — FOC driver cost killer at 12 joints
- N20 gearmotor, AS5600, TMC2209, 2208/2804 gimbal — hobby India SKUs for the study only

## IMU and current

- MPU-6050 GY-521 — v1 IMU ([Robocraze](https://robocraze.com/products/mpu-6050-triple-axis-accelerometer-gyroscope-module))
- ACS712 20/30 A — servo-rail sense; not INA219 3.2 A

## Audio on Pi 5

- Pi 5 has no 3.5 mm analog jack; use USB or I2S for mic and speaker.
