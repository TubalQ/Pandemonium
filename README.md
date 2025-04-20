# Pandemonium

**Pandemonium** is a lightweight, configurable honeypot service written in Python. It is designed to slow down, observe, and optionally block unwanted network scanners and brute-force attempts by occupying their connections indefinitely or by enforcing a connection threshold that results in firewall-level blocking.

This tool is inspired from [endlessh](https://github.com/skeeto/endlessh), while offering enhanced customization and written in Python.

## Features

- Endless mode: keeps connections open indefinitely.
- Block mode: blocks repeat offenders using `iptables` after configurable threshold.
- Full logging: stores all events to both plaintext and CSV.
- GeoIP integration (optional): log country/city using MaxMind GeoLite2.
- Resource limits: restrict CPU and memory usage.
- Self-contained setup: generates configs, logs, helper scripts, and guide on first run.
- Script-driven operation: start, stop, reset, and status scripts generated automatically.
- Docker and systemd friendly.

## Installation


git clone https://github.com/TubalQ/Pandemonium.git
cd Pandemonium
sudo ./pandemonium


On first run, Pandemonium will:

- Create `/opt/Pandemonium` with all runtime files.
- Set up a Python virtual environment and install dependencies.
- Generate helper scripts:
  - `startpandemonium.sh`
  - `stop-pandemonium.sh`
  - `reset-blocks.sh`
  - `status-pandemonium.sh`
- Generate `config.txt` and `guide.txt`.

## Configuration

Edit `/opt/Pandemonium/config.txt` to control behavior.

### Example


PORT=2222
MODE=endless
DELAY=10000
MAX_LINE_LENGTH=32
MAX_CONNECTIONS=3
TIMEOUT=0
MAX_CLIENTS=0
CPU_LIMIT=50
MEMORY_LIMIT=100
LOGFILE=pandemonium.log
CSV_LOG=events.csv
DB=GeoLite2-City.mmdb
```

### Parameters

| Key               | Description                                                             |
|-------------------|-------------------------------------------------------------------------|
| `PORT`            | Port to listen on                                                       |
| `MODE`            | `endless` (keep alive) or `block` (block IP after threshold)            |
| `DELAY`           | Delay (ms) between lines in endless mode                                |
| `MAX_LINE_LENGTH` | Length of each sent line in endless mode                                |
| `MAX_CONNECTIONS` | Number of allowed connections before blocking (in block mode)           |
| `TIMEOUT`         | Optional timeout in seconds (0 = no timeout)                            |
| `MAX_CLIENTS`     | Max concurrent connections (0 = unlimited)                              |
| `CPU_LIMIT`       | Soft CPU usage limit (%)                                                |
| `MEMORY_LIMIT`    | Max memory usage (MB, soft limit)                                       |
| `LOGFILE`         | Log file for plaintext events                                           |
| `CSV_LOG`         | CSV file for structured logs                                            |
| `DB`              | Path to GeoLite2 database file (optional)                               |

## Commands


# Start Pandemonium
sudo /opt/Pandemonium/startpandemonium.sh

# Stop Pandemonium
sudo /opt/Pandemonium/stop-pandemonium.sh

# View status
sudo /opt/Pandemonium/status-pandemonium.sh

# Reset iptables rules and clear blocked IPs
sudo /opt/Pandemonium/reset-blocks.sh


## Logs

- Text log: `/opt/Pandemonium/pandemonium.log`
- CSV log: `/opt/Pandemonium/events.csv`
- Blocked IPs: `/opt/Pandemonium/blocked_ips.txt`
- User guide: `/opt/Pandemonium/guide.txt`

## GeoIP (optional)

To enable IP geolocation:

1. Download `GeoLite2-City.mmdb` from MaxMind  
2. Place the file in `/opt/Pandemonium/`
3. Set the path in `config.txt`:


DB=GeoLite2-City.mmdb


## Requirements

- Python 3.7+
- Root privileges (required for iptables)
- `iptables` installed
- Optional: [MaxMind GeoLite2 database](https://dev.maxmind.com/geoip/geolite2/)

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for more details.
