# System Architecture Diagram

```mermaid
graph TD
    %% External Data Sources
    subgraph "Data Sources"
        API[External APIs<br/>DOE APIMS, OpenAQ<br/>OpenWeatherMap<br/>NASA POWER, Google/Waze]
        USER[User Inputs<br/>Onboarding & Profile<br/>City/District<br/>Commute mode<br/>Work/Sleep hours<br/>Sunscreen/mask habits]
        DENGUE[iDengue<br/>Dengue hotspots<br/>Weekly cases<br/>Outbreak data<br/>State statistics]
    end
    
    %% Data Processing
    INGESTION[Data Ingestion & Validation<br/>API calls, cleansing<br/>normalization, resampling<br/>form validation]
    
    %% Storage Layers
    subgraph "Data Storage"
        RELATIONAL[(Relational DB<br/>PostgreSQL/MySQL<br/>Historical + User profile)]
        CACHE[(Caching Layer<br/>Redis<br/>Real-time API Responses)]
        ARCHIVE[(Archiving Layer<br/>Cold storage<br/>Raw + processed data)]
    end
    
    %% Core Engine
    ENGINE[Digital Twin Engine<br/>Environment + Health computation]
    
    %% User Interface
    UI[User Interface<br/>Avatar, Health Vitals<br/>Alerts, Visualizations]
    
    %% Data Flow Connections
    API -->|APIs/JSON| INGESTION
    USER -->|Form JSON| INGESTION
    DENGUE -->|Dengue Data/JSON| INGESTION
    
    INGESTION -->|Normalized + Profile| RELATIONAL
    INGESTION -->|Real-time feed| CACHE
    INGESTION -->|Raw & processed snapshots| ARCHIVE
    
    RELATIONAL -->|Historical + profile| ENGINE
    CACHE -->|Low-latency data| ENGINE
    ARCHIVE -->|Archived reference| ENGINE
    
    ENGINE -->|Visualization & nudges| UI
    UI -->|Preference updates| ENGINE
    
    %% Styling
    classDef dataSource fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef processing fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef storage fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef engine fill:#fce4ec,stroke:#880e4f,stroke-width:2px
    classDef interface fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    
    class API,USER,DENGUE dataSource
    class INGESTION processing
    class RELATIONAL,CACHE,ARCHIVE storage
    class ENGINE engine
    class UI interface
```

## Data Flow Description

### Input Sources:
1. **External APIs**: Environmental data from various sources (air quality, weather, traffic)
2. **User Inputs**: Personal profile and preferences from users
3. **iDengue**: Dengue fever data including hotspots, weekly cases, and outbreak statistics

### Processing Pipeline:
- **Data Ingestion & Validation**: Central processing unit that cleans, normalizes, and validates all incoming data

### Storage Architecture:
- **Relational DB**: Stores historical data and user profiles
- **Caching Layer**: Provides real-time data with low latency
- **Archiving Layer**: Long-term storage for raw and processed data

### Core Engine:
- **Digital Twin Engine**: Performs environment and health computations using all available data sources

### User Interface:
- **User Interface**: Displays visualizations, health vitals, alerts, and receives user preference updates

## Key Benefits of Adding iDengue:
- **Health Risk Assessment**: Integrates dengue outbreak data for location-based health alerts
- **Comprehensive Health Monitoring**: Combines environmental and disease data for better health insights
- **Proactive Alerts**: Can warn users about dengue hotspots in their area
- **Data-Driven Decisions**: Provides historical dengue data for trend analysis
