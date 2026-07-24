# Thermal Authorization System Architecture

This repository contains the software and machine learning pipeline for our privacy-focused thermal authorization system using the MLX90640 sensor and Raspberry Pi 5.

## System Block Diagram

```mermaid
graph TD
    subgraph Physical_Input ["1. Physical Input Layer"]
        HumanSubject(Human Subject radiates IR) --> MLXSensor
        MLXSensor[[MLX90640 32x24 Array]] -->|Hardware I2C Stream| RaspberryPi
    end

    subgraph Raspberry_Pi ["2. Raspberry Pi 5 Core Processing"]
        direction TB
        
        RaspberryPi[Python Core Script] -->|Process Raw Matrix| DataPreprocessing[Data Preprocessing]
        
        DataPreprocessing -->|1. Background Subtraction| Thresh
        DataPreprocessing -->|2. Iso. Human Temp Span| Thresh
        DataPreprocessing -->|3. Normalize 0.0-1.0| Thresh[[Processed Normalized Matrix]]
        
        Thresh --> FeatureExtraction[Geometric Feature Extraction]
        FeatureExtraction -->|32x24 Heat Matrix| TFLite
        
        subgraph AI_Authorization_Engine ["3. AI Authorization Engine"]
            TFLite[TFLite / ML Classifier] -->|Query Feature Vector| DB
            DB[(Local authorized DB)]
            TFLite -->|Match Confidence %| ThresholdCheck{Confidence >= 90%?}
            
            ThresholdCheck -->|YES| Access Granted
            ThresholdCheck -->|NO| Access Denied
        }

        subgraph Monitoring_Visualization ["4. Monitoring & Visualization"]
            Thresh -->|Live Processed Matrix| VisualGen[Generate Heatmap Image]
            VisualGen -->|Colormap Applied e.g. 'Jet'| UI(User Interface App)
            
            Access Granted -->|State: Success| UI
            Access Denied -->|State: Failed| UI
            
            UI -->|Render System State & Heatmap| Screen[Physical Screen]
            UI -->|Prompt User Override| ManualAuth{Manual Override Required?}
            ManualAuth -->|User Input: Allow| Access Granted
            ManualAuth -->|User Input: Deny| Access Denied
        }
    end

    subgraph Physical_Outputs ["5. Physical Outputs (Actuation)"]
        Access Granted -->|GPIO Trigger HIGH| Relay[[12V Relay Module]]
        Relay -->|Switch 12V Power| Solenoid(Solenoid Door Lock Open)
        
        Access Denied -->|Log Failed Attempt| DataPreprocessing
    end
```
