flowchart TD
    %% 1. Physical Sensing Layer
    subgraph Physical_Sensing["1. PHYSICAL SENSING LAYER"]
        A["Human Subject in Front of MLX90640"] --> B["Captures 24x32 Thermal Matrix"]
    end

    %% 2. Core Processing
    subgraph Pi_Processing["2. RASPBERRY PI 5 COMPUTATION"]
        B -->|I2C Stream @ 400kHz| C["Read & Normalize Raw Temp Values"]
        C --> D["Identify Session Type:<br/>(Pre-Game Baseline / Post-Game Assessment)"]
    end

    %% 3. Local AI Engine
    subgraph AI_Engine["3. LOCAL AI / ML ENGINE"]
        D --> E["PyTorch/ONNX CNN Feature Extraction"]
        E --> F["Compare Post-Game to Baseline Vector"]
        F --> G["Compute Delta Matrix (Post - Pre)"]
    end

    %% Decision Node
    G --> H{"Is Thermal Assessment<br/>Confidence High?"}

    %% 4. Decision Branches
    subgraph Decision_Handling["4. ASSESSMENT VERIFICATION"]
        H -- YES --> I["4A. AUTOMATIC CONFIRMATION<br/>- Finalize recovery score<br/>- Flag muscle asymmetry/risk"]
        H -- NO --> J["4B. MANUAL REVIEW FLAG<br/>- Prompt kiosk override<br/>- Request re-scan or physio check"]
    end

    %% 5. Output and Logging
    subgraph Output_Logging["5. OUTPUT & LOGGING STAGE"]
        I --> K[("LOCAL POSTGRESQL DATABASE<br/>- Log fatigue/risk scores<br/>- Store Delta Matrix data<br/>- Update athlete profile")]
        I --> L["UI & COACH'S DASHBOARD<br/>- Render thermal heatmaps<br/>- Display recovery metrics<br/>- Trigger status indicator"]
        
        J --> K
        J --> L
    end

    %% Styling
    style Physical_Sensing fill:#f9f9f9,stroke:#333,stroke-width:1px
    style Pi_Processing fill:#f9f9f9,stroke:#333,stroke-width:1px
    style AI_Engine fill:#f9f9f9,stroke:#333,stroke-width:1px
    style Decision_Handling fill:#f9f9f9,stroke:#333,stroke-width:1px
    style Output_Logging fill:#f9f9f9,stroke:#333,stroke-width:1px
    
    style H fill:#fff9c4,stroke:#fbc02d,stroke-width:2px
    style I fill:#c8e6c9,stroke:#388e3c,stroke-width:2px
    style J fill:#ffcdd2,stroke:#d32f2f,stroke-width:2px
    style K fill:#bbdefb,stroke:#1976d2,stroke-width:2px
    style L fill:#d1c4e9,stroke:#512da8,stroke-width:2px
    
