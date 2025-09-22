# Complete System Architecture Diagram

```mermaid
graph TD
    %% External Data Sources
    subgraph "External Data Sources"
        API[External APIs<br/>DOE APIMS, OpenAQ<br/>OpenWeatherMap<br/>NASA POWER, Google/Waze]
        DENGUE[iDengue<br/>Dengue hotspots<br/>Weekly cases<br/>Outbreak data<br/>State statistics]
    end
    
    %% User Data Sources
    subgraph "User Data Sources"
        USER[User Inputs<br/>Onboarding & Profile<br/>City/District<br/>Commute mode<br/>Work/Sleep hours<br/>Sunscreen/mask habits]
        HEALTH[Health Metrics Collection<br/>Apple HealthKit (iOS)<br/>Health Connect (Android)<br/>Steps, Sleep, HRV, RHR<br/>Active Energy, Mindful Sessions<br/>Respiratory Rate, Workouts]
    end
    
    %% Data Processing
    INGESTION[Data Ingestion & Validation<br/>API calls, cleansing<br/>normalization, resampling<br/>form validation<br/>health data processing]
    
    %% Storage Layers
    subgraph "Data Storage"
        RELATIONAL[(Relational DB<br/>PostgreSQL/MySQL<br/>Historical + User profile<br/>+ Health snapshots)]
        CACHE[(Caching Layer<br/>Redis<br/>Real-time API Responses<br/>+ Recent health data)]
        ARCHIVE[(Archiving Layer<br/>Cold storage<br/>Raw + processed data<br/>+ Historical health trends)]
    end
    
    %% Core Engine
    ENGINE[Digital Twin Engine<br/>Environment + Health computation<br/>+ Personal health analysis<br/>+ Risk assessment]
    
    %% User Interface
    UI[User Interface<br/>Avatar, Health Vitals<br/>Alerts, Visualizations<br/>+ Health dashboard<br/>+ Personalized insights]
    
    %% Data Flow Connections
    API -->|APIs/JSON| INGESTION
    DENGUE -->|Dengue Data/JSON| INGESTION
    USER -->|Form JSON| INGESTION
    HEALTH -->|Health Metrics/JSON| INGESTION
    
    INGESTION -->|Normalized + Profile + Health| RELATIONAL
    INGESTION -->|Real-time feed + Health| CACHE
    INGESTION -->|Raw & processed snapshots| ARCHIVE
    
    RELATIONAL -->|Historical + profile + health| ENGINE
    CACHE -->|Low-latency data + health| ENGINE
    ARCHIVE -->|Archived reference + trends| ENGINE
    
    ENGINE -->|Visualization & nudges<br/>+ Health insights| UI
    UI -->|Preference updates<br/>+ Health goals| ENGINE
    
    %% Styling
    classDef externalSource fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef userSource fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef processing fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef storage fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef engine fill:#fce4ec,stroke:#880e4f,stroke-width:2px
    classDef interface fill:#f1f8e9,stroke:#33691e,stroke-width:2px
    
    class API,DENGUE externalSource
    class USER,HEALTH userSource
    class INGESTION processing
    class RELATIONAL,CACHE,ARCHIVE storage
    class ENGINE engine
    class UI interface
```

## Enhanced Data Flow Description

### **Data Sources (4 Categories):**

1. **External APIs**: Environmental data (air quality, weather, traffic)
2. **iDengue**: Dengue fever data (hotspots, weekly cases, outbreak statistics)
3. **User Inputs**: Personal profile and preferences from users
4. **Health Metrics Collection**: Real-time health data from mobile devices

### **Health Metrics Integration:**
- **iOS**: Apple HealthKit integration
- **Android**: Health Connect integration
- **Metrics Collected**: Steps, Sleep, HRV, Resting Heart Rate, Active Energy, Mindful Sessions, Respiratory Rate, Workouts
- **Storage**: Local encrypted SQLite database (`health_snapshots` table)

### **Enhanced Processing Pipeline:**
- **Data Ingestion & Validation**: Now includes health data processing and validation
- **Real-time Health Monitoring**: Continuous collection and processing of health metrics
- **Health Data Normalization**: Standardizing health data across iOS/Android platforms

### **Enhanced Storage Architecture:**
- **Relational DB**: Now stores health snapshots alongside historical and profile data
- **Caching Layer**: Includes recent health data for low-latency access
- **Archiving Layer**: Stores historical health trends for long-term analysis

### **Enhanced Digital Twin Engine:**
- **Personal Health Analysis**: Processes individual health metrics
- **Risk Assessment**: Combines environmental, dengue, and personal health data
- **Predictive Health Modeling**: Uses historical health trends for insights

### **Enhanced User Interface:**
- **Health Dashboard**: Dedicated health metrics visualization
- **Personalized Insights**: Health recommendations based on all data sources
- **Health Goal Tracking**: Monitor progress against personal health targets

## Key Benefits of Adding Health Metrics:

1. **Comprehensive Health Monitoring**: Combines environmental, disease, and personal health data
2. **Personalized Risk Assessment**: Individual health factors in risk calculations
3. **Proactive Health Alerts**: Warns users about health risks based on their personal data
4. **Health Trend Analysis**: Tracks long-term health patterns and correlations
5. **Personalized Recommendations**: Tailored advice based on individual health profiles
6. **Real-time Health Insights**: Immediate feedback on health metrics and environmental impacts

This creates a truly comprehensive digital twin that considers environmental factors, disease risks, and personal health metrics for holistic health management.
