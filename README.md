# Zabbix Template for Deva Broadcast DB4005

## Overview
This repository contains a Zabbix template for monitoring the **Deva Broadcast DB4005** FM monitoring receiver. The template allows you to easily integrate the device with Zabbix and collect key metrics for real-time monitoring and alerting.

## Features
- **Device Availability**: Monitors whether the DB4005 is online or offline.
- **FM Signal Parameters**: Collects key parameters such as RF level, audio deviation, MPX power, pilot level, and more.
- **Audio Metrics**: Monitors left and right audio levels, loudness, and multipath signal.
- **RDS Metrics**: Monitors RDS PI, PS, PTY, and RT.
- **Temperature Monitoring**: Monitors the temperature of the device's motherboard.

## Requirements
- **Zabbix Server Version**: Tested with Zabbix 6.0 and above.
- **Deva Broadcast DB4005**: The device should have SNMP enabled, and it must be accessible from the Zabbix server.
- **SNMP Configuration**: Ensure that the DB4005 has SNMP community strings configured, and the Zabbix server has the necessary permissions to query it.

## Installation
1. **Import the Template**:
   - Download the `db4005_template.xml` file from this repository.
   - Go to the **Zabbix frontend**: Configuration → Templates → Import.
   - Select the downloaded XML file and click **Import**.

2. **Assign the Template**:
   - Go to **Configuration → Hosts**.
   - Select the host that represents your DB4005 device.
   - Link the **DB4005 template** to this host.

3. **Configure SNMP Interface**:
   - Ensure the host representing the DB4005 has an SNMP interface configured.
   - Verify the IP address, port (default is 161), and community string.

## Template Details
The template includes the following metrics:

- **RF Level**: Monitor the strength of the incoming FM signal (`[Signal] RF Tuner`).
- **Audio Levels**: Left and right channel audio deviation (`[LEFT] AudioSignal`, `[RIGHT] AudioSignal`).
- **MPX Power**: Composite MPX power level (`[Audio] MPX Power`).
- **Pilot and RDS Levels**: Pilot signal level (`[Pilot] RF Tuner`), RDS PI (`[RDS] PI`), PS (`[RDS] PS`), PTY (`[RDS] PTY`), and RT (`[RDS] RT`).
- **Multipath Signal**: Monitor multipath interference (`[MultiPath] RF Tuner`).
- **Loudness**: Audio signal loudness (`[Loudness] Audio Signal`).
- **Temperature**: Monitor motherboard temperature (`[Temp] MotherBoard`).

### Triggers
- **Frequency [MSK]**: Triggered if the frequency is outside the desired range (`last(/DEVA DB4005/DB4005.mntrFreq,#1)>=100.5`).
- **LOST AUDIO. Average level < -40dB FS**: Triggered if average audio level falls below -40dB (`avg(min(/DEVA DB4005/DB4005.Left.Audio.Signal,2m))<=-40 or avg(min(/DEVA DB4005/DB4005.Right.Audio.Signal,2m))<=-40`).
- **Loudness (LUFS) [MSK]**: Triggered if loudness level is less than or equal to -12 LUFS (`last(/DEVA DB4005/DB4005.Loudness.Momentary,#2)<=-12`).
- **MULTIPATH [MSK]**: Triggered if multipath value is greater than or equal to 15 (`avg(min(/DEVA DB4005/DB4005.Mpath.ValueAvr,2m))>=15`).
- **RDS PI [MSK]**: Triggered if RDS PI value is outside the desired range (`avg(last(/DEVA DB4005/DB4005.mntrRdsPi))>=7745`).
- **RDS PS [MSK]**: Triggered if RDS PS value is not equal to 'ZHARA FM' (`avg(last(/DEVA DB4005/DB4005.mntrRdsPs))<>'ZHARA FM'`).
- **Temperature [MSK]**: Triggered if motherboard temperature exceeds 45°C (`last(/DEVA DB4005/DB4005.TempValue,#2)>45`).

## Usage
- This template is designed for broadcast engineers and technicians who need to monitor the **Deva Broadcast DB4005** for quality assurance and system uptime.
- You can customize the thresholds and alerting conditions to meet your specific operational needs.

## License
This template is released under the **MIT License**. You are free to use, modify, and distribute it, provided that you retain the original copyright notice.

## Feedback
If you encounter issues or have suggestions for improvement, feel free to open an issue or submit a pull request. Contributions are welcome!

## Acknowledgements
- **Deva Broadcast** for the DB4005 FM monitoring receiver.
- **Zabbix Community** for providing excellent monitoring capabilities.

