```mermaid
erDiagram
  USER ||--|| PROFILE : "has"
  USER ||--o{ HEALTH_SNAPSHOT : "records"
  USER ||--o{ ALERT : "receives"

  STATE ||--o{ DISTRICT : "contains"
  DISTRICT ||--o{ AIR_QUALITY_READING : "has"
  DISTRICT ||--o{ WEATHER_READING : "has"
  DISTRICT ||--o{ DENGUE_HOTSPOT : "has"
  DISTRICT ||--o{ ALERT : "location (optional)"

  STATE ||--o{ STATE_WEEKLY_DENGUE : "tracks"

  ALERT_RULE ||--o{ ALERT : "triggers"

  USER {
    integer user_id PK
    text external_id
    text created_at
  }
  
  PROFILE {
    integer user_id PK, FK
    text name
    text gender
    text skin_tone
    text commute_mode
    real sleep_target_hours
  }
  
  STATE {
    text state_code PK
    text state_name
  }
  
  DISTRICT {
    integer district_id PK
    text district_name
    text state_code FK
  }
  
  AIR_QUALITY_READING {
    integer aq_id PK
    integer district_id FK
    text timestamp
    integer aqi
    real pm25
    real pm10
    real no2
    real o3
    text source
  }
  
  WEATHER_READING {
    integer weather_id PK
    integer district_id FK
    text timestamp
    real temp_c
    real humidity
    real uv_index
  }
  
  DENGUE_HOTSPOT {
    integer hotspot_id PK
    integer district_id FK
    text locality_name
    text start_date
    text end_date
    text source
  }
  
  STATE_WEEKLY_DENGUE {
    integer swd_id PK
    text state_code FK
    integer year
    integer epi_week
    integer weekly_cases
    integer weekly_deaths
    integer total_hotspots_in_state
    UNIQUE(state_code, year, epi_week)
  }
  
  HEALTH_SNAPSHOT {
    integer health_id PK
    integer user_id FK
    text timestamp
    real sleep_hours
    integer resting_hr
    integer steps
    real stress_score
  }
  
  ALERT_RULE {
    integer rule_id PK
    text alert_type
    text threshold_json
    integer enabled
  }
  
  ALERT {
    integer alert_id PK
    integer user_id FK
    integer district_id FK
    integer rule_id FK
    text alert_type
    text severity
    text message
    text created_at
    integer acknowledged
  }
```

## Key Relationships

1. **USER** has a one-to-one relationship with **PROFILE**
2. **USER** can have multiple **HEALTH_SNAPSHOT** records
3. **USER** can receive multiple **ALERT**s
4. **STATE** contains multiple **DISTRICT**s
5. **DISTRICT** has multiple **AIR_QUALITY_READING**s, **WEATHER_READING**s, and **DENGUE_HOTSPOT**s
6. **STATE** tracks multiple **STATE_WEEKLY_DENGUE** records
7. **ALERT_RULE** can trigger multiple **ALERT**s
8. **ALERT** is optionally associated with a **DISTRICT** (location)

## Entity Descriptions

- **USER**: Core user entity with external ID and creation timestamp
- **PROFILE**: User profile information including health preferences
- **STATE/DISTRICT**: Geographic hierarchy for location-based data
- **AIR_QUALITY_READING**: Environmental air quality measurements
- **WEATHER_READING**: Weather conditions and UV index data
- **DENGUE_HOTSPOT**: Dengue fever outbreak locations
- **STATE_WEEKLY_DENGUE**: Weekly dengue statistics at state level
- **HEALTH_SNAPSHOT**: User health metrics over time
- **ALERT_RULE**: Configurable rules for generating alerts
- **ALERT**: Generated alerts for users based on rules and conditions
