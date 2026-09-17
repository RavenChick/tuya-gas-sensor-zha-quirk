# Tuya TS0601 Gas Sensor ZHA Quirk

Custom Zigbee Home Automation (ZHA) quirk for the Tuya TS0601 Smart Gas Leak Detector (`_TZE204_chbyv06x`) in Home Assistant.

## Problem Statement
The Tuya TS0601 gas detector identifies itself as a `SMART_PLUG` via standard Zigbee descriptors, causing Home Assistant to expose incorrect entities and fail to report gas leak alarms triggered by Tuya's proprietary `0x0402` cluster attribute.

## Solution
This custom quirk written in Python:
- Overrides the endpoint profile from `SMART_PLUG` to `IAS_ZONE`.
- Implements `TuyaGasDetectorCluster` to parse Tuya Data Point `0x0402`.
- Uses an internal `Bus` listener to map gas status flags directly to standard `IasZone.ZoneStatus.Alarm_1` events.

## Installation
1. Copy `ts0601_gas.py` to your Home Assistant custom quirks directory (e.g., `/config/custom_zha_quirks/`).
2. Add the custom quirks path to your `configuration.yaml`:
   ```yaml
   zha:
     custom_quirks_path: /config/custom_zha_quirks/

Restart Home Assistant and re-pair the gas sensor device.

Tech Stack
Language: Python 3

Libraries: zigpy, zhaquirks

Platform: Home Assistant (ZHA)
