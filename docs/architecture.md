```mermaid

graph LR
    %% --- Data Sources ---
    subgraph Data Sources
        WS[Weather Services API\n(Forecast and Historical)]
        LS[Local Sensors\n(Meteo Station, Sky Camera)]
        DCS[Data Center Sensors\n(IT Load, Temp, Humidity, BMS/DCIM)]
        AS[ARCT Sensors\n(Surface Temp, Tilt, Status, Degradation?)]
    end

    %% --- Data Processing & Storage ---
    subgraph Data Processing and Storage
        DI[Data Ingestion & Validation]
        DB[(Data Storage\nTime-Series DB / Data Lake)]
    end

    %% --- AI Core ---
    subgraph AI Core
        TP[Model: Tilt Prediction\n(Weather, Load -> Angle)]
        SA[Model: Sky Analysis\n(Image -> Clear Sky % / Direction)]
        HM[Model: Health Monitoring\n(ARCT Sensors -> Anomaly/Maint. Need)]
        MD[Model: Material Degradation\n(History, UV -> Lifespan/Efficiency)]
        %% Placement Optimization is offline/design time, not shown in real-time loop
        %% PO[Model: Placement Optimization (Design)]

        %% Central Brain
        subgraph AI Orchestrator
            CTRL[AI Control Logic / Orchestrator]
        end
    end

    %% --- Control & Actuation ---
    subgraph Control and Actuation
        AA[ARCT Actuators\n(Tilt Motors)]
        ECS[Existing Cooling Systems\n(via BMS/DCIM: CRACs, Chillers, Liquid)]
        BMS[(BMS / DCIM Interface)]
    end

    %% --- Physical Environment ---
    subgraph Physical Environment
        ARCT[ARCT Units (Physical)]
        DC[Data Center Environment\n(Internal Temperature)]
    end

    %% --- Outputs & Monitoring ---
    subgraph Outputs and Monitoring
        DASH[Reporting & Visualization Dashboard]
        USER[Operator / User]
        ALERT[Alerting System]
        MT[(Maintenance Planning)]
    end

    %% --- Offline Processes ---
    subgraph Offline Processes
        TRAIN[Model Retraining & Updates]
    end

    %% --- Connections ---

    %% Data Ingestion Flow
    WS --> DI
    LS --> DI
    DCS --> DI
    AS --> DI
    DI --> DB

    %% AI Models reading data
    DB --> TP
    DB --> SA  %% Historical data for training/context
    LS -- Sky Camera Image --> SA %% Real-time image feed
    DB --> HM
    AS --> HM %% Real-time sensor feed
    DB --> MD

    %% AI Models feeding the Orchestrator
    TP --> CTRL
    SA --> CTRL
    HM -- Current Status/Efficiency --> CTRL
    MD -- Predicted Efficiency --> CTRL
    DCS -- Real-time Load/Temp --> CTRL %% Direct feed for immediate response
    WS -- Real-time/Short-term Forecast --> CTRL

    %% Orchestrator sending commands
    CTRL -- Tilt Commands --> AA
    CTRL -- Cooling Mode Commands --> BMS

    %% Control influencing physical systems
    AA --> ARCT
    BMS --> ECS

    %% Physical systems influencing environment & sensors (Feedback Loop)
    ARCT -- Cooling Effect --> DC
    ECS -- Cooling Effect --> DC
    DC -- Measured Temp --> DCS
    ARCT -- Measured State --> AS

    %% Reporting and User Interaction
    DB --> DASH
    CTRL -- System Status --> DASH
    HM -- Anomaly Alerts --> ALERT
    MD -- Degradation Forecast --> MT
    HM -- Maintenance Needs --> MT
    ALERT --> USER
    DASH --> USER
    USER -- Queries/Overrides? --> CTRL %% Optional override path

    %% Offline Training Loop
    DB -- Historical Data --> TRAIN
    TRAIN -- Updated Models --> TP
    TRAIN -- Updated Models --> SA
    TRAIN -- Updated Models --> HM
    TRAIN -- Updated Models --> MD

    %% Style definitions
    classDef datasrc fill:#cde4ff,stroke:#333,stroke-width:2px;
    classDef dataproc fill:#ccffcc,stroke:#333,stroke-width:2px;
    classDef aicore fill:#fff0cc,stroke:#333,stroke-width:2px;
    classDef control fill:#ffcccc,stroke:#333,stroke-width:2px;
    classDef physical fill:#e0e0e0,stroke:#333,stroke-width:2px;
    classDef output fill:#e6ccff,stroke:#333,stroke-width:2px;
    classDef offline fill:#f5f5f5,stroke:#666,stroke-width:1px,stroke-dasharray: 5 5;

    class WS,LS,DCS,AS datasrc;
    class DI,DB dataproc;
    class TP,SA,HM,MD,CTRL aicore;
    class AA,ECS,BMS control;
    class ARCT,DC physical;
    class DASH,USER,ALERT,MT output;
    class TRAIN offline;
```
