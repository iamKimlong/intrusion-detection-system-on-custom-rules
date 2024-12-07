# Network Intrusion Detection System (IDS)

A custom Intrusion Detection System (IDS) developed in Python for detecting network intrusions such as port scans, DDoS attacks, brute-force attempts, and DoS attacks. This IDS provides real-time monitoring, alerting, and logging capabilities, allowing administrators to identify and respond to potential threats effectively.

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Testing the IDS](#testing-the-ids)
- [Modules Overview](#modules-overview)
- [License](#license)

## Features

- **Real-time Packet Sniffing**: Captures network packets using Scapy.
- **Custom Rule Engine**: Detects suspicious activities based on predefined rules.
- **Alerting System**: Generates alerts for detected intrusions.
- **Menu-Driven Interface**: Allows users to interact with the IDS through a console menu.
- **Logging**: Logs all activities and alerts to `ids.log`.
- **Extensibility**: Easily add new detection rules and alerting mechanisms.
- **Configuration files**: located at ./config/config.py

## Requirements

- Python 3.x
- Administrative privileges (for packet sniffing)

## Installation

1. **Clone the Repository**

   ```bash
   git clone https://github.com/yourusername/ids.git
   cd ids
   ```

2. **Install Required Libraries**

   Install the necessary Python packages:

   ```bash
   pip install scapy loguru
   ```

   > **Note:** Depending on your system, you may need to use `pip3` instead of `pip`.

3. **Ensure Administrative Privileges**

   Packet sniffing requires root or administrative privileges. Make sure you run the IDS with the necessary permissions.

## Configuration

### 1. Create the `config.txt` File

Create a `config.txt` file in the project directory with the following content:

```ini
# Rules
[Rule]
id=1
name=Port Scan Detection
description=Detects multiple connection attempts to different ports
action=alert
protocol=TCP
src_ip=any
dst_port=any
threshold_count=10
threshold_interval=60

[Rule]
id=2
name=DDoS Detection
description=Detects high number of packets to a single IP
action=alert
protocol=any
src_ip=any
dst_ip=your_server_ip
threshold_count=1000
threshold_interval=60

# Alert Channels
[AlertChannel]
type=console

# General Settings
interface=eth0
bpf_filter=tcp or udp
```

- Replace `your_server_ip` with your actual server IP address.
- Adjust the `interface` to match your network interface (e.g., `eth0`, `wlan0`).

### 2. Update Alert Channels (Optional)

By default, alerts are logged to the console and `ids.log`. You can extend the `AlertingSystem` class to add email or SMS notifications.

## Usage

Run the IDS using the following command:

```bash
sudo python3 main.py
```

> **Note:** Running as root is necessary for packet sniffing.

### Menu Options

After starting the IDS, you will see the following menu:

```
Network Intrusion Detection System
1. Start IDS Monitoring
2. View Current Rules
3. View Recent Alerts
4. System Status
5. Exit
Select an option:
```

- **Option 1**: Starts the IDS monitoring. Press Enter to stop monitoring.
- **Option 2**: Displays the currently active detection rules.
- **Option 3**: Shows the most recent alerts generated by the IDS.
- **Option 4**: Displays the system status, including whether monitoring is active, the number of loaded rules, and total alerts.
- **Option 5**: Exits the IDS.

## Testing the IDS

To ensure the IDS is functioning correctly, you can simulate attacks using tools like `nmap` and `hping3`.

### 1. Simulate a Port Scan

Use `nmap` to simulate a port scan:

```bash
nmap -p 1-1000 <target_ip>
```

- Replace `<target_ip>` with the IP address of the machine running the IDS.

### 2. Simulate a DDoS Attack

Use `hping3` to simulate a DDoS attack:

```bash
hping3 --flood -p 80 -S <target_ip>
```

- Ensure `hping3` is installed on your system (`sudo apt-get install hping3` on Debian-based systems).
- Replace `<target_ip>` with the IP address of the machine running the IDS.

### 3. Check for Alerts

After running the simulations:

1. Go back to the IDS menu.
2. Choose **Option 3** to view recent alerts.
3. Verify that alerts corresponding to the simulated attacks are displayed.

## Modules Overview

### 1. `main.py`

- Entry point of the application.
- Handles user interaction through the `MenuHandler` class.

### 2. `ids_system.py`

- Integrates the packet sniffer, rule engine, and traffic monitor.
- Manages starting and stopping of the IDS components.

### 3. `packet_sniffer.py`

- Captures network packets using Scapy.
- Processes packets and forwards them to the `RuleEngine`.

### 4. `rule_engine.py`

- Contains the logic for checking packets against detection rules.
- Generates alerts when rules are violated.

### 5. `rules.py`

- Defines the base `Rule` class and specific rule implementations like `PortScanRule` and `DDoSRule`.
- Each rule contains logic to detect a specific type of intrusion.

### 6. `alerting.py`

- Manages alert generation and logging.
- Stores alerts in memory and logs them to `ids.log`.

### 7. `models.py`

- Contains shared data classes and enums, such as `NetworkPacket`, `Alert`, `AlertType`, and `AlertSeverity`.

### 8. `config.py`

- Handles loading and parsing of the configuration from `config.txt`.
- Stores rules, alert channels, and general settings.

### 9. `traffic_monitor.py`

- Manages the status of the traffic monitoring.
- Can be extended for additional traffic analysis features.

## License

This project is licensed under the GNU General Public License v3.0. See the LICENSE file for details.

---

**Disclaimer:** This IDS is intended for educational purposes and should be used responsibly. Unauthorized network scanning or intrusion detection on networks without permission is illegal and unethical.