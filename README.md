# Sports Thermography Kiosk - Architecture Flowcharts

This document outlines the detailed system architecture and the high-level operational flow for the Edge-AI Sports Recovery Kiosk.

---

## 1. High-Level Abstract Flowchart

This condensed flowchart highlights the 5 main operational stages of the kiosk system.

```mermaid
flowchart TD
    A["1. SENSING & DATA INPUT<br/>(MLX90640 Thermal Sensor Array)"] --> B["2. RASPBERRY PI 5 CORE<br/>(Data Preprocessing & ROI)"]
    B --> C["3. LOCAL AI ENGINE (CNN)<br/>(Thermal Delta & Feature Vector)"]
    C --> D["4. DATABASE LOGGING<br/>(PostgreSQL Baseline & Sessions)"]
    C --> E["5. KIOSK & COACH APP<br/>(Heatmap & Recovery Score)"]

    style A fill:#e1f5fe,stroke:#0288d1,stroke-width:2px
    style B fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style C fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style D fill:#e8f5e9,stroke:#388e3c,stroke-width:2px
    style E fill:#e8eaf6,stroke:#303f9f,stroke-width:2px
