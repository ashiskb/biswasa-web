---
title: 'Drone DatCon 3.6.1 -- V3 .CSV description'
date: 2023-09-18
permalink: /posts/2023/09/biswas/blog-datcon/
tags:
  - laravel
---

I have compiled a short list of variables from the `DatCon 3.6.1 (7/30/2018)` [V3 .CSV column format](https://datfile.net/DatCon/fieldsV3.html). Please note, the following variables are either absent in the data files I worked with, or I removed due privacy concerns.

* Removed the following columns:
  * `IMU_ATTI(0):Longitude`
  * `IMU_ATTI(0):Latitude`
  * `IMU_ATTI(0):distanceHP`
  * `IMU_ATTI(0):distanceTravelled`
  * `IMU_ATTI(1):Longitude`
  * `IMU_ATTI(1):Latitude`
  * `IMU_ATTI(1):distanceHP`
  * `IMU_ATTI(1):distanceTravelled`
  * `eventLog`
  * `GPS(0):Long`
  * `GPS(0):Lat`
  * `IMUEX(0):rtk_Longitude`
  * `IMUEX(0):rtk_Latitude`
  * `IMUEX(1):rtk_Longitude`
  * `IMUEX(1):rtk_Latitude`

* I have added a column `Operating_Phase` :
    * Possible values of this column are {'Resting', 'Taking-Off', 'Hovering', 'Climbing', 'Landing'}

* PDF was generated with `pandoc 2.14.2`
* **Please note:** not all signals could be cataloged nor properly described here, but nor should you ignore them for these reasons.
* Alright, the catalog of `V3 .CSV columns` goes here:

| No. | Name | Description | Frequency (Hz) |
| :- | :-------------  | :--------         |  :-:          |
| 1   | Tick# | Internal bus clock | platform dependent |
| 2   | offsetTime | Elapsed time | variable |
| 3   | IMU_ATTI(0):numSats | Number of Satellites | |
| 4   | IMU_ATTI(0):barometer:Raw | Meters. Raw data from barometer. | 200 |
| 5   | IMU_ATTI(0):barometer:Smooth | Meteres. Smoothed barometer data | 200 |
| 6   | IMU_ATTI(0):accel:X | Meters per second. Acceleration along X axis | 200 |
| 7   | IMU_ATTI(0):accel:Y | Meters per second. Acceleration along Y axis | 200 |
| 8  | IMU_ATTI(0):accel:Z | Meters per second. Acceleration along Z axis | 200 |
| 9  | IMU_ATTI(0):accel:Composite | Meters per second. $\sqrt{\text{accel}^2_X + \text{accel}^2_Y + \text{accel}^2_Z}$ | 200 |
| 10  | IMU_ATTI(0):gyro:X | Degrees per second. Rotation about the X axis | 200 |
| 11  | IMU_ATTI(0):gyro:Y | Degrees per second. Rotation about the Y axis | 200 |
| 12  | IMU_ATTI(0):gyro:Z | Degrees per second. Rotation about the Z axis | 200 |
| 13  | IMU_ATTI(0):gyro:Composite | Degrees per second.  $\sqrt{\text{gyro}^2_X + \text{gyro}^2_Y + \text{gyro}^2_Z}$| 200 |
| 14  | IMU_ATTI(0):mag:X | Micro Tesla. Magnetometer reading about the X axis| 50 |
| 15  | IMU_ATTI(0):mag:Y | Micro Tesla. Magnetometer reading about the X axis | 50 |
| 16  | IMU_ATTI(0):mag:Z | Micro Tesla. Magnetometer reading about the X axis | 50 |
| 17  | IMU_ATTI(0):mag:Mod | Micro Tesla.  $\sqrt{\text{mag}^2_X + \text{mag}^2_Y + \text{mag}^2_Z}$| 50 |
| 18  | IMU_ATTI(0):velN | Meters per second. Velocity North | 200 |
| 19  | IMU_ATTI(0):velE | Meters per second. Velocity East | 200 |
| 20  | IMU_ATTI(0):velD | Meters per second. Velocity Down | 200 |
| 21  | IMU_ATTI(0):velComposite | Meters per second. $\sqrt{\text{Vel}^2_N + \text{Vel}^2_E + \text{Vel}^2_D}$ | 200 |
| 22  | IMU_ATTI(0):velH | Meters per second. Horizontal velocity, that is: $\sqrt{\text{Vel}^2_N + \text{Vel}^2_E}$ | 200 |
| 23  | IMU_ATTI(0):GPS-H | Meters per second. Difference between velocity computed from successive GPS coordinates and horizontal velocity computed from IMU sensors (Vel:Horizontal, $\text{Vel}_H$) | 200 |
| 24  | IMU_ATTI(0):roll | Degrees. Note, the yaw value will be corrected for geomagnetic declination after GPS data is valid, i.e., yaw will be true and not magnetic | 200 |
| 25  | IMU_ATTI(0):pitch | Degrees. Note, the yaw value will be corrected for geomagnetic declination after GPS data is valid, i.e., yaw will be true and not magnetic | 200 |
| 26  | IMU_ATTI(0):yaw | Degrees. Note, the yaw value will be corrected for geomagnetic declination after GPS data is valid, i.e., yaw will be true and not magnetic | 200 |
| 27  | IMU_ATTI(0):yaw360 | Degrees. Range 0 - 360 | 200 |
| 28  | IMU_ATTI(0):totalGyro:Z | Degrees. Integration and summation of Gyro:Z. Can be used to compute Gyro:Z error. Also, useful for checking roll, pitch, and yaw values coming from flight controller (FC) | 200 |
| 29  | IMU_ATTI(0):totalGyro:X | Degrees. Integration and summation of Gyro:X. Can be used to compute Gyro:X error. Also, useful for checking roll, pitch, and yaw values coming from flight controller (FC) | 200 |
| 30  | IMU_ATTI(0):totalGyro:Y | Degrees. Integration and summation of Gyro:Y. Can be used to compute Gyro:Y error. Also, useful for checking roll, pitch, and yaw values coming from flight controller (FC) | 200 |
| 31  | IMU_ATTI(0):magYaw | Yaw value computed from magnetometers and corrected with pitch and roll. Not the same as Yaw which comes from the Flight controller (FC) | 200 |
| 32  | IMU_ATTI(0):directionOfTravel[mag] | Degrees. Range = [-180,180]. Computed from successive latitude/longitude coordinates. Not corrected with local geomagnetic declination., i.e., value can be compared against P3 yaw. | 1 |
| 33  | IMU_ATTI(0):directionOfTravel[true] | Degrees. Range = [-180,180]. Computed from successive latitude/longitude coordinates. Corrected with local geomagnetic declination., i.e., value can not be compared against P3 yaw. | 1 |
| 34  | IMU_ATTI(0):temperature | IMU temp. Steady state 65 degree celsius | 200 |
| 35  | General:relativeHeight | Meters. Altitude above Home Point | 10 |
| 36  | General:absoluteHeight | Meters. Populated if the Home Point elevation has been set | 200 |
| 37  | GPS(0):Date | Integer that contains date, e.g., 20171003 means 2017-10-03 GMT | 5 |
| 38  | GPS(0):Time | Integer that contains time, e.g., 100334 means 10:03:34 GMT | 5 |
| 39  | GPS:dateTimeStamp | alternate name: dateTime. DateTime in ISO-8601 format. Not available in CsvView | 5 |
| 40  | GPS(0):heightMSL | Meters. height above mean sea level | 5 |
| 41  | GPS(0):hDOP | Horizontal dilution of precision. Units unknown. | 5 |
| 42  | GPS(0):pDOP | Position dilution of precision. Units unknown. | 5 |
| 43  | GPS(0):sAcc | Some kind of accuracy measure. |  |
| 44  | GPS(0):numGPS | Number of GPS satellites | 5 | 
| 45  | GPS(0):numGLNAS | Number of GLONAS satellites | 5 |
| 46  | GPS(0):numSV | Total number of satellites | 5 |
| 47  | GPS(0):velN | Meters per second. Velocity North | 200 |
| 48  | GPS(0):velE | Meters per second. Velocity East | 200 |
| 49  | GPS(0):velD | Meters per second. Velocity Down | 200 |
| 50  | RC:Aileron | Range [-10000, 10000] Neutral = 0. Stick left or down = -10000. Stick right or up = 10000 | 50 |
| 51  | RC:Elevator | Range [-10000, 10000] Neutral = 0. Stick left or down = -10000. Stick right or up = 10000 | 50 |
| 52  | RC:Rudder | Range [-10000, 10000] Neutral = 0. Stick left or down = -10000. Stick right or up = 10000 | 50 |
| 53  | RC:Throttle | Range [-10000, 10000] Neutral = 0. Stick left or down = -10000. Stick right or up = 10000 | 50 |
| 54  | Controller:navHealth | Same as General:gpsHealth. Useful when looking at a tablet .DAT | 50 |
| 55  | Controller:ctrl_level | Unknown, maybe a gpsHealth for the RC | 50 |
| 56  | OA:frontDistance | | 10 |
| 57  | Motor:Speed:RFront | Actual Motor speed. RPM | 50 |
| 58  | Motor:Speed:LFront | Actual Motor speed. RPM | 50 |
| 59  | Motor:Speed:LBack | Actual Motor speed. RPM | 50 |
| 60  | Motor:Speed:RBack | Actual Motor speed. RPM | 50 |
| 61  | Motor:EscTemp:RFront | ESC temperature, not motor temperature | 50 |
| 62  | Motor:EscTemp:LFront | ESC temperature, not motor temperature | 50 |
| 63  | Motor:EscTemp:LBack | ESC temperature, not motor temperature | 50 |
| 64  | Motor:EscTemp:RBack | ESC temperature, not motor temperature | 50 |
| 65  | Motor:PPMrecv:RFront | | 50 |
| 66  | Motor:PPMrecv:LFront |  | 50 |
| 67  | Motor:PPMrecv:LBack |  | 50 |
| 68  | Motor:PPMrecv:RBack |  | 50 |
| 69  | Motor:PPMsend:RFront |  | 50 |
| 70  | Motor:PPMsend:LFront |  | 50 |
| 71  | Motor:PPMsend:LBack |  | 50 |
| 72  | Motor:PPMsend:RBack |  | 50 |
| 73  | Motor:V_out:RFront |  | 50 |
| 74  | Motor:V_out:LFront |  | 50 |
| 75  | Motor:V_out:LBack |  | 50 |
| 76  | Motor:V_out:RBack |  | 50 |
| 77  | Motor:Volts:RFront |  | 50 |
| 78  | Motor:Volts:LFront |  | 50 |
| 79  | Motor:Volts:LBack |  | 50 |
| 80  | Motor:Volts:RBack |  | 50 |
| 81  | Motor:Current:RFront |  | 50 |
| 82  | Motor:Current:LFront |  | 50 |
| 83  | Motor:Current:LBack |  | 50 |
| 84  | Motor:Current:RBack |  | 50 |
| 85  | Motor:Status:RFront | 0 = Normal, other values unknown | 50 |
| 86  | Motor:Status:LFront | 0 = Normal, other values unknown | 50 |
| 87  | Motor:Status:LBack | 0 = Normal, other values unknown | 50 |
| 88  | Motor:Status:RBack | 0 = Normal, other values unknown | 50 |
| 89  | Motor:thrustAngle | Degrees. Computed from motor speeds. Direction the A/C is being pushed by the motors. Relative to the A/C, not the inertial frame. | 200 |
| 90  | AirComp:AirSpeedBody:X | These fields are not fully understood | 5 |
| 91  | AirComp:AirSpeedBody:Y | These fields are not fully understood | 5 |
| 92  | AirComp:Alti | These fields are not fully understood | 5 |
| 93  | AirComp:VelNorm | These fields are not fully understood | 5 |
| 94 | AirComp:VelTime:1 | These fields are not fully understood | 5 | 
| 95 | AirComp:VelTime:2 | These fields are not fully understood | 5 |
| 96 | AirComp:VelLevel | These fields are not fully understood | 5 |
| 97 | IMU_ATTI(1):numSats| | |
| 98 | IMU_ATTI(1):barometer:Raw | Meters. Raw data from barometer. | 200|
| 99 | IMU_ATTI(1):barometer:Smooth | Meters. Smoothed barometer data | 200 |
| 100 | IMU_ATTI(1):accel:X | Meters per second. Acceleration along the X axis | 200 |
| 101 | IMU_ATTI(1):accel:Y | Meters per second. Acceleration along the Y axis | 200 |
| 102 | IMU_ATTI(1):accel:Z | Meters per second. Acceleration along the Z axis | 200 |
| 103 | IMU_ATTI(1):accel:Composite | Meters per second. $\sqrt{\text{accel}^2_X + \text{accel}^2_Y + \text{accel}^2_Z}$ | 200 |
| 104 | IMU_ATTI(1):gyro:X | Degrees per second. Rotation about the X axis | 200 |
| 105 | IMU_ATTI(1):gyro:Y | Degrees per second. Rotation about the Y axis | 200 |
| 106 | IMU_ATTI(1):gyro:Z | Degrees per second. Rotation about the Z axis | 200 |
| 107 | IMU_ATTI(1):gyro:Composite | Degrees per second. $\sqrt{\text{gyro}^2_X + \text{gyro}^2_Y + \text{gyro}^2_Z}$ | 200 |
| 108 | IMU_ATTI(1):mag:X | | 50 |
| 109 | IMU_ATTI(1):mag:Y | | 50 |
| 110 | IMU_ATTI(1):mag:Z | | 50 |
| 111 | IMU_ATTI(1):mag:Mod | $\sqrt{\text{mag}^2_X + \text{mag}^2_Y + \text{mag}^2_Z}$| 50 | 
| 112 | IMU_ATTI(1):velN | Meters per second. Velocity North | 200 |
| 113 | IMU_ATTI(1):velE | Meters per second. Velocity East | 200 |
| 114 | IMU_ATTI(1):velD | Meters per second. Velocity Down | 200 |
| 115 | IMU_ATTI(1):velComposite | Meters per second. $\sqrt{\text{Vel}^2_N + \text{Vel}^2_E + \text{Vel}^2_D}$ | 200 |
| 116 | IMU_ATTI(1):velH | Meters per second. Horizontal Velocity. $\sqrt{\text{Vel}^2_N + \text{Vel}^2_E}$ | 200 |
| 117 | IMU_ATTI(1):GPS-H | Meters per second. Difference between velocity computed from successive GPS coordinates and horizontal velocity computed from IMU sensors (Vel:Horizontal, $\text{Vel}_H$) | 200 |
| 118 | IMU_ATTI(1):roll | Degrees. Note, the yaw value will be corrected for geomagnetic declination after GPS data is valid, i.e., yaw will be true and not magnetic | 200 |
| 119 | IMU_ATTI(1):pitch | Degrees. Note, the yaw value will be corrected for geomagnetic declination after GPS data is valid, i.e., yaw will be true and not magnetic | 200 |
| 120 | IMU_ATTI(1):yaw | Degrees. Note, the yaw value will be corrected for geomagnetic declination after GPS data is valid, i.e., yaw will be true and not magnetic | 200 |
| 121 | IMU_ATTI(1):yaw360 | Degrees. Range 0 - 360 | 200 |
| 122 | IMU_ATTI(1):totalGyro:Z | Degrees. Integration and summation of Gyro:Z. Can be used to compute Gyro:Z error. Also, useful for checking roll, pitch, and yaw values coming from flight controller (FC) | 200 |
| 123 | IMU_ATTI(1):totalGyro:X | Degrees. Integration and summation of Gyro:X. Can be used to compute Gyro:Z error. Also, useful for checking roll, pitch, and yaw values coming from flight controller (FC) | 200 |
| 124 | IMU_ATTI(1):totalGyro:Y | Degrees. Integration and summation of Gyro:Y. Can be used to compute Gyro:Z error. Also, useful for checking roll, pitch, and yaw values coming from flight controller (FC) | 200 |
| 125 | IMU_ATTI(1):magYaw | Yaw value computed from magnetometers and corrected with pitch and roll. Not the same as Yaw which comes from the Flight controller (FC) | 200 |
| 126 | IMU_ATTI(1):directionOfTravel[mag] | Degrees. Range = [-180, 180]. Computed from successive latitude/longitude coordinates. Not corrected with local geomagnetic declination. i.e., value can be compared against P3 yaw. | 1 |
| 127 | IMU_ATTI(1):directionOfTravel[true] | Degrees. Range = [-180, 180]. Computed from successive latitude/longitude coordinates. Corrected with local geomagnetic declination. i.e., value can not be compared against P3 yaw. | 1 |
| 128 | IMU_ATTI(1):temperature | IMU temp. Steady state = 65 degree celsius. | 200 |
| 129 | IMUEX(0):err | None, SPEED_LARGE_ERROR, GPS_YAW_ERROR | 200 |
| 130 | IMUEX(0):rtk_Alti | | 200 |
| 131 | IMUEX(1):err | None, SPEED_LARGE_ERROR, GPS_YAW_ERROR | 200 |
| 132 | IMUEX(1):rtk_Alti | | 200 |
| 133 | MotorCtrl:PWM:RFront | Pulse width modulation. Can be used to determine commanded motor speed. Range 0-100% | 50 |
| 134 | MotorCtrl:PWM:LFront | Pulse width modulation. Can be used to determine commanded motor speed. Range 0-100% | 50 |
| 135 | MotorCtrl:PWM:LBack | Pulse width modulation. Can be used to determine commanded motor speed. Range 0-100% | 50 |
| 136 | MotorCtrl:PWM:RBack | Pulse width modulation. Can be used to determine commanded motor speed. Range 0-100% | 50 |
| 137 | AirCraftCondition:fsmState | | 50 |
| 138 | AirCraftCondition:nearGround | | 50 |
| 139 | AirCraftCondition:landState | | 50 |
| 140 | AirCraftCondition:UP_acc_t | | 50 |
| 141 | AirCraftCondition:UP_TF_t | | 50 |
| 142 | Operating_Phase | operating phase of the drone at the moment (i.e., at Tick#) | Variable |


## References (as of 9/6/2023)
* `DatCon 3.6.1 (7/30/2018)` [V3 .CSV column format](https://datfile.net/DatCon/fieldsV3.html)