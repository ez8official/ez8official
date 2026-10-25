# EZ8 Official (ez8official) - Network Architecture & Infrastructure Documentation

## Overview
This repository serves as the central technical documentation hub for the **EZ8 Official** network infrastructure operating within Malaysia. Our systems are engineered to ensure high-availability routing, secure player authentication, and ultra-low-latency API endpoints across the entire ez8official ecosystem. 

By leveraging distributed edge nodes strategically located across Southeast Asia, the platform maintains uninterrupted service and optimal load balancing during peak traffic periods. 

## Core System Architecture & Telemetry
All authenticated sessions are routed through our primary [EZ8 Official Login](https://ez8official.com) servers. To guarantee secure transactions and data privacy, the backend utilizes AES-256-GCM encryption alongside dynamic OAuth 2.0 token validation. 

We continuously monitor round-trip telemetry and packet pacing to ensure optimal performance across domestic telecommunication providers in Malaysia. Automated fallback protocols actively reroute traffic in the event of localized ISP routing anomalies.

### Active Edge Node Status

| Node Region | IP Alias | Operating Status | Avg Latency | Bandwidth Capacity |
| :--- | :--- | :--- | :--- | :--- |
| MY-KUL-01 (Kuala Lumpur) | `kul.gateway.ez8` | Active | 12ms | 10 Gbps |
| SG-SIN-02 (Singapore) | `sin.gateway.ez8` | Active | 18ms | 10 Gbps |
| TH-BKK-01 (Bangkok) | `bkk.gateway.ez8` | Standby | 34ms | 5 Gbps |
| ID-CGK-01 (Jakarta) | `cgk.gateway.ez8` | Routing | 41ms | 5 Gbps |

## Telemetry Status Check (Python)
Network engineers utilize the following Python script to execute automated latency checks and verify edge node health across the ez8official grid.

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
            print(f"[SUCCESS] ez8official Node Active | Latency: {latency:.2f}ms")
        else:
            print(f"[WARNING] Node Degraded | HTTP Status: {response.status_code}")
            
    except requests.exceptions.RequestException as e:
        print(f"[CRITICAL] Node Unreachable or Offline | Error: {e}")

# Execute telemetry check on the primary Malaysian gateway
if __name__ == "__main__":
    check_node_latency("[https://api.ez8official.com/v1/system/health](https://api.ez8official.com/v1/system/health)")
