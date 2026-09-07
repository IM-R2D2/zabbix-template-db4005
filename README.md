# Zabbix Template for Deva Broadcast DB4005

## Overview
This repository contains a Zabbix template for monitoring the **Deva Broadcast DB4005** FM monitoring receiver. The template allows you to easily integrate the device with Zabbix and collect key metrics for real-time monitoring and alerting.

## Features
- **Device Availability**: Monitors whether the DB4005 is online or offline.
- **FM Signal Parameters**: Collects key parameters such as RF level, audio deviation, MPX power, pilot level, and more.
- **Audio Metrics**: Monitors left and right audio levels, loudness, and multipath signal.
- **RDS Metrics**: Monitors RDS PI, PS, PTY, and RT.
- **Temperature Monitoring**: Monitors the temperature of the device's motherboard.
- **SD Card Monitoring**: Monitors SD card usage and status.

## Requirements
- **Firmware Version**: Compatible with Deva Broadcast DB4005 firmware version 2.6.2203 (!) and above.
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

4. **Set station values**:
   - In the linked template, set `{$DB4005.EXPECTED.FREQUENCY}`, `{$DB4005.EXPECTED.RDS.PI}`, and `{$DB4005.EXPECTED.RDS.PS}` to the frequency and RDS identity of the monitored service.
   - The bundled macro values retain the original example station values and must be changed before enabling the corresponding alerts.

## Template Details
The template includes the following metrics:

- **RF Level**: Monitor the strength of the incoming FM signal (`[Tuner] RF Signal`).
- **Audio Levels**: Left and right channel audio deviation (`[Audio] Left Channel`, `[Audio] Right Channel`).
- **MPX Power**: Composite MPX power level (`[Audio] MPX Power`).
- **Pilot and RDS Levels**: Pilot signal level (`[Tuner] Pilot`), RDS PI (`[RDS] PI`), PS (`[RDS] PS`), PTY (`[RDS] PTY`), and RT (`[RDS] RT`).
- **Multipath Signal**: Monitor multipath interference (`[Tuner] MultiPath`).
- **Loudness**: Audio signal loudness (`[Audio] Loudness`).
- **Temperature**: Monitor motherboard temperature (`[Device] Temperature`).
- **SD Card Monitoring**: Monitors SD card usage (`[Device] SD Card Free`, `[Device] SD Card Used`) and status (`[SD Card] Broken`).
- **Device Status**: Includes fan speed (`[Device] FanSpeed`) and other alarms (e.g., `[Status] Alarm MPX`, `[Status] Alarm RF`, `[Status] Alarm Temp`).

### Triggers
- **Frequency**: Triggered if the frequency differs from `{$DB4005.EXPECTED.FREQUENCY}`.
- **LOST AUDIO. Average level < -40dB FS**: Triggered if average audio level falls below -40dB.
- **Loudness (LUFS)**: Triggered if loudness level is less than or equal to -12 LUFS.
- **MULTIPATH**: Triggered if multipath value.
- **RDS PI / PS**: Triggered if the received RDS identity differs from the configured station macros.
- **Temperature**: Triggered if motherboard temperature.
- **SD card capacity**: Free and used capacity are collected. The DB4005 MIB does not expose an SD-card health state, so the template deliberately does not fabricate a failure alarm from capacity counters.

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
