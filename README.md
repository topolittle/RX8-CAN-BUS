# RX-8 Common CAN BUS codes
Here is the common CAN BUS codes used for telemetry in a typical racing application.  
TODO: Found a way to get the brake pressure instead of an on/off signal.

# Equipement used
The following codes have been successfully tested with the following equipement and software:
- [OBDLink MX+](https://www.obdlink.com/products/obdlink-mxp) Bluetooth scanner
- [Huawei P30 Pro](https://consumer.huawei.com/en/phones/p30) mobile Phone with Android 10
- Mazda RX-8 2006 (serie 1), manual transmission and DSC equiped
- [RaceChrono](https://racechrono.com) v7.5.3

# Initial setup
As of version 7.5.3, it's required to enable experimental features in RaceChrono to make it works. Follow this [link](https://racechrono.com/article/faq/how-do-i-log-can-bus-messages) for more information.

# The codes
### Accelerator Position
- PID: `0x201`
- Bytes offset: `6`
- Bytes length: `1`
- Bits mask: `None`
- Value type: `UINT8`
- Formula: `Accelerator position percent  = Value / 2`
- RaceChrono item: `Accelerator position (%)`
- RaceChrono formula: `BytesToUInt(raw, 6, 1) / 2`
- Note: The raw value has a range from `0` to `200`

### Brake pedal
- PID: `0x212`
- Bytes offset: `5`
- Bytes length: `1`
- Bits mask: `0x08`
- Value type: `Bool`
- Formula: `Brake depressed = Value`
- RaceChrono item: `Brake position (%)`
- RaceChrono formula: `BitsToUInt(raw, 44, 1) * 100`

### Hand brake
- PID: `0x212`
- Bytes offset: `4`
- Bytes length: `1`
- Bits mask: `0x40`
- Value type: `Bool`
- Formula: `Hand brake engaged = Value`
- RaceChrono item: `Brake position (%) + postfix 2`
- RaceChrono formula: `BitsToUInt(raw, 33, 1) * 100`

### Neutral
- PID: `0x231`
- Bytes offset: `0`
- Bytes length: `1`
- Bits mask: `0x80`
- Value type: `Bool`
- Formula: `In neutral position = ~Value`
- RaceChrono item: `Clutch position (%)`
- RaceChrono formula: `(1 – BitsToUInt(raw, 0, 1)) * 100`
- Note: `0` when in neutral or clutch depressed, otherwise `1`

### Coolant temperature
- PID: `0x420`
- Bytes offset: `0`
- Bytes length: `1`
- Bits mask: `None`
- Value type: `UINT8`
- Formula: `Temp (°C) = Value – 40`
- RaceChrono item: `Coolant temperature (°F)`
- RaceChrono formula: `BytesToUint(raw, 0, 1) – 40`
- Note: RaceChrono converts the value to Fahrenheit automatically.

### Engine RPM
- PID: `0x201`
- Bytes offset: `0`
- Bytes length: `2`
- Bits mask: `None`
- Value type: `UINT16`
- Formula: `RPM  = Value / 4.0`
- RaceChrono item: `Engine RPM (rpm)`
- RaceChrono formula: `BytesToUint(raw, 0, 2) / 3.85`
- Note: Divide by 4 is the standard formula, however 3.85 seems to match the value displayed by the car’s instrument cluster. Don't know wich value is the good one, needs investigation.

### Intake temperature
- PID: `0x250`
- Bytes offset: `3`
- Bytes length: `1`
- Bits mask: `None`
- Value type: `UINT8`
- Formula: `Temp (°C) = Value – 40`
- RaceChrono item: `Intake temperature (°F)`
- RaceChrono formula: `BytesToUInt(raw, 3, 1) – 40`
- Note: RaceChrono convert it to Fahrenheit automatically.

### Vehicle speed (KM/h)
- PID: `0x201`
- Bytes offset: `4`
- Bytes length: `2`
- Bits mask: `None`
- Value type: `UINT16`
- Formula: `Speed (KM/h) = (Value – 10000) / 100.0`
- RaceChrono item: `Speed (KM/h)`
- RaceChrono formula: `(BytesToUInt(raw, 4, 2) – 10000) / 360`
- Note: RaceChrono needs the value in M/s, instead of dividing by 100 we have to divide by 360 to achieve that.

### Steering Angle (Degrees)
- PID: `0x81`
- Bytes offset: `2`
- Bytes length: `2`
- Bits mask: `None`
- Value type: `INT16`
- Formula: `Stering Angle (°) = Value`
- RaceChrono item: `Steering angle(°)`
- RaceChrono formula: `BytesToInt(raw, 2, 2)`
