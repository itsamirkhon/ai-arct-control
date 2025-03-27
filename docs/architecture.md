```mermaid

graph TD
    %% ==== Simplified and Enhanced Architecture ====
    
    %% --- Data Layer ---
    subgraph Data_Layer["📊 Data Layer"]
        DS1[("🌤️ Weather API")]
        DS2[("📷 Sky Camera")]
        DS3[("🌡️ DC Sensors")]
        DS4[("⚙️ ARCT Sensors")]
    end

    %% --- Processing ---
    subgraph Processing["🖥️ Data Processing"]
        DI[("🧩 Ingestion")]
        DB[(("💾 Database"))]
    end

    %% --- AI/ML Models ---
    subgraph AI_Core["🧠 AI Core"]
        direction TB
        M1[["Tilt Optimizer"]]
        M2[["Sky Analyzer"]]
        M3[["Health Monitor"]]
        M4[["Degradation Predictor"]]
        CTRL[[("🤖 Orchestrator")]]
    end

    %% --- Control ---
    subgraph Control["🎛️ Control"]
        AA[("🔄 ARCT Actuators")]
        CS[("❄️ Cooling Systems")]
    end

    %% --- Physical ---
    subgraph Physical["🏢 Physical World"]
        ARCT[["ARCT Panels"]]
        DC[["Data Center"]]
    end

    %% --- UI/Outputs ---
    subgraph UI["📱 Monitoring"]
        DASH[("📈 Dashboard")]
        ALERT[("🚨 Alerts")]
        MT[("📅 Maintenance")]
    end

    %% ==== Cleaner Connections ====
    DS1 & DS2 & DS3 & DS4 --> DI --> DB

    DB --> M1 & M2 & M3 & M4
    DS2 --> M2  %% Camera direct feed
    DS4 --> M3  %% ARCT sensors direct

    M1 & M2 & M3 & M4 --> CTRL
    DS3 & DS1 --> CTRL  %% Real-time feeds

    CTRL --> AA & CS
    AA --> ARCT
    CS --> DC
    ARCT & DC --> DS3 & DS4  %% Feedback loop

    DB --> DASH
    CTRL --> DASH
    M3 --> ALERT & MT
    M4 --> MT

    DASH & ALERT --> U((("👩💻 Operator")))

    %% ==== Styling ====
    classDef dataLayer fill:#E3F2FD,stroke:#42A5F5
    classDef processing fill:#E8F5E9,stroke:#66BB6A
    classDef ai fill:#FFF3E0,stroke:#FFA726
    classDef control fill:#FFEBEE,stroke:#EF5350
    classDef physical fill:#E0E0E0,stroke:#757575
    classDef ui fill:#F3E5F5,stroke:#AB47BC

    class Data_Layer,DS1,DS2,DS3,DS4 dataLayer
    class Processing,DI,DB processing
    class AI_Core,M1,M2,M3,M4,CTRL ai
    class Control,AA,CS control
    class Physical,ARCT,DC physical
    class UI,DASH,ALERT,MT ui
```
