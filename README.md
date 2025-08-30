

# P8-Live data streaming dashboard of temperature sensor

**Live streaming of room temperature data with end-to-end IoT pipeline: from sensor to dashboard visualization.**

***

## Project Overview

This project demonstrates a full-stack IoT data pipeline to **collect, process, store, and visualize** live room temperature readings. Using a Raspberry Pi with a thermistor sensor, data flows through MQTT to both MySQL and InfluxDB databases and is visualized in Power BI (for SQL) or Grafana (for InfluxDB).

**Pipeline Summary:**

- **Sensor:** Thermistor on Raspberry Pi
- **Data Transfer:** MQTT (Local Mosquitto or HiveMQ Cloud)
- **Processing:** Node-Red
- **Storage:** MySQL (for Power BI) & InfluxDB (for Grafana)
- **Visualization:** Power BI, Grafana dashboards

***

## Objectives

1. **Hardware Setup:** Raspberry Pi with thermistor sensor
2. **Software Setup:** Configure Pi, Node-Red pipeline, and database connections
3. **Data Flow:** Publish temperature values over MQTT; extract, transform, and load into MySQL/InfluxDB
4. **Visualization:** Live dashboards via Power BI (SQL) or Grafana (InfluxDB)

***

## Pipeline Architecture

```
[Sensor (Thermistor, Pi)]
         │
         ▼
[MQTT Publish (Python)]
         │
         ▼
        (LAN/Cloud)
         │
         ▼
[MQTT Broker (Mosquitto/HiveMQ)]
         │
         ▼
[Node-RED Pipeline]
    • Ingest (MQTT)
    • Transform (JSON → Table/Timeseries)
    • Load (MySQL/InfluxDB)
         │
         ▼
[Database]
         │
         ▼
[Dashboard (Power BI/Grafana)]
```

- **Real-time streaming:** Data published every 5–30 seconds
- **Low-latency, scalable, and decoupled design**
- **ETL via Node-Red:** Extract → Transform → Load for both SQL and time-series DB

***

## Setup Instructions

### 1. Raspberry Pi (Publisher)

- Install and configure **Mosquitto** (local broker) or use **HiveMQ Cloud**
- Hardware setup: Thermistor sensor connected to GPIO
- Python code to read temperature and publish JSON payload:
  ```json
  {"timestamp":"YYYY-MM-DD HH:MM:SS", "temperature":25.89}
  ```
- Setup Python virtual environment and run:
  ```bash
  source myenv/bin/activate
  python3 11.1.1_Thermometer/publish_temperature.py
  ```

### 2. Node-Red (Subscriber/ETL)

- **MQTT In node:** Subscribe to the relevant topic (e.g., `room/temperature`)
- **Function node:** Parse and clean JSON, select data fields, format for DB
- **MySQL/InfluxDB Out node:** Configure server connections, database/table or bucket
- For **InfluxDB Cloud/Cloud SQL:**
  - Use API tokens and cloud URLs in node configs
  - Set up a bucket (InfluxDB) or database/table (MySQL)

### 3. Firewall/Network

- Ensure MQTT and DB ports are allowed on both Raspberry Pi and PC
- Example for opening ports in Windows Firewall (8080/TCP):
  ```shell
  netsh advfirewall firewall add rule name="Open Port 8080" dir=in action=allow protocol=TCP localport=8080
  ```

### 4. Visualization

- **Power BI:** SQL database connection (periodic refresh)
- **Grafana:** InfluxDB as a data source; use Flux query language for dashboard panels
- Set auto-refresh to 10 seconds in Grafana

***

## Features & Benefits

- **Real-time telemetry streaming**
- **Event-driven, decoupled design** (sensor doesn’t need to know DB details)
- **Flexible ETL:** Easily add filters, alerts, or transform logic in Node-Red
- **Scalable:** Supports multiple sensor topics/devices
- **Resilient:** MQTT QoS and persistent DB storage

***

## Recommendations & Extensions

- Replace Node-Red ETL with REST API-based ingestion (for more advanced control)
- Dockerize the pipeline for portability and scaling
- Add alerting (e.g., high/low temperature emails/SMS)
- Support for additional sensor types or more sophisticated analytics

***

## Security

- Only open required network ports
- Use proper profiles/rules in firewall configuration
- Use API tokens or user authentication for all cloud services

***

## References

- [Mosquitto MQTT](https://mosquitto.org/)
- [HiveMQ Cloud](https://www.hivemq.com/)
- [Node-RED](https://nodered.org/)
- [MySQL](https://www.mysql.com/)
- [InfluxDB](https://www.influxdata.com/)
- [Grafana](https://grafana.com/)
- [Power BI](https://powerbi.microsoft.com/)

***

**Master thesis inspiration:** Bringing research in Industry 4.0 to hands-on IoT real-world projects!

***

Feel free to copy, adapt, and improve this template as needed for your GitHub repository. If you want files like `pipeline_diagram.png` or code samples, mention them in the appropriate sections!
