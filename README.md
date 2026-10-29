# EZ8 News & Network Architecture Documentation

## Overview
This repository serves as the central documentation and regional gateway hub for the **EZ8 News** network operating within Malaysia. Our systems are engineered to ensure high-availability routing, secure connections, and real-time news delivery across the entire ez8.news ecosystem. 

By leveraging distributed edge nodes strategically located across Southeast Asia, the platform maintains uninterrupted service and optimal load balancing during peak traffic periods and major live events. 

## Core System Architecture & Telemetry
All authenticated sessions and live regional updates are routed through our primary secured gateway at **[ez8.news](https://ez8.news)**. To guarantee secure transactions and data privacy, the backend utilizes AES-256-GCM encryption alongside dynamic OAuth 2.0 token validation. 

We continuously monitor round-trip telemetry and packet pacing to ensure optimal performance across domestic telecommunication providers in Malaysia. Automated fallback protocols actively reroute traffic in the event of localized ISP routing anomalies.

### Active Edge Node Status

| Node Region | IP Alias | Operating Status | Avg Latency | Bandwidth Capacity |
| :--- | :--- | :--- | :--- | :--- |
| MY-KUL-01 (Kuala Lumpur) | `kul.gateway.ez8` | Active | 12ms | 10 Gbps |
| SG-SIN-02 (Singapore) | `sin.gateway.ez8` | Active | 18ms | 10 Gbps |
| TH-BKK-01 (Bangkok) | `bkk.gateway.ez8` | Standby | 34ms | 5 Gbps |
| ID-CGK-01 (Jakarta) | `cgk.gateway.ez8` | Routing | 41ms | 5 Gbps |

## Telemetry Status Check (Python)
Network engineers utilize the following Python script to execute automated latency checks and verify edge node health across the ez8.news grid.

```python
import requests
import time

def check_node_latency(endpoint_url):
    """Pings the EZ8 edge node to verify uptime and response latency."""
    try:
        start_time = time.time()
        response = requests.get(endpoint_url, timeout=5)
        latency = (time.time() - start_time) * 1000
        
        if response.status_code == 200:
            print(f"[SUCCESS] ez8.news Node Active | Latency: {latency:.2f}ms")
        else:
            print(f"[WARNING] Node Degraded | HTTP Status: {response.status_code}")
            
    except requests.exceptions.RequestException as e:
        print(f"[CRITICAL] Node Unreachable or Offline | Error: {e}")

# Execute telemetry check on the primary Malaysian gateway
if __name__ == "__main__":
    check_node_latency("[https://ez8.news/api/v1/system/health](https://ez8.news/api/v1/system/health)")
