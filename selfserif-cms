# SelfBrief CMS Complete Data Structure

## System Overview Architecture

```mermaid
graph TB
    subgraph "Core System"
        User[User System]
        Auth[Authentication & Roles]
        Geo[Geographic Data]
    end
    
    subgraph "Business Entities"
        Org[Organisations]
        People[People Management]
        Aircraft[Aircraft Types]
    end
    
    subgraph "Operations"
        Briefs[Briefing System]
        Airports[Airport Management]
        Categories[Airport Categories]
    end
    
    User --> Auth
    Auth --> Org
    Org --> People
    Org --> Aircraft
    Org --> Airports
    Airports --> Categories
    Org --> Briefs
    Briefs --> Aircraft
    Briefs --> Airports
```

## Core Models Entity Relationship Diagram

```mermaid
erDiagram
    User ||--o| Person : "linked to"
    User }o--o{ Role : "has roles"
    User ||--o{ UserConnection : "tracks connections"
    
    Person ||--o| PersonDetails : "has current"
    Person ||--o{ PersonDetails : "has history"
    Person }o--o{ AircraftType : "qualified for"
    PersonDetails }o--|| PersonTitle : "has title"
    
    Person ||--o{ OrganisationPersonPosition : "holds positions"
    OrganisationPersonPosition }o--|| Organisation : "at organisation"
    OrganisationPersonPosition }o--|| PersonRole : "in role"
    
    User {
        int id PK
        string email UK
        boolean is_staff
        boolean is_active
        boolean is_cms_notifications_enabled
        datetime date_joined
    }
    
    Role {
        int id PK
        string name
        string codename UK
    }
    
    Person {
        int id PK
        string ofac_api_id
        datetime created_at
    }
    
    PersonDetails {
        int id PK
        int person_id FK
        string first_name
        string middle_name
        string last_name
        string abbreviated_name
        string contact_email
        string contact_phone
        int title_id FK
        date change_effective_date
    }
```

## Organisation Structure

```mermaid
erDiagram
    Organisation ||--|| OrganisationDetails : "has current"
    Organisation ||--o{ OrganisationDetails : "has history"
    Organisation ||--o| OrganisationSettings : "has settings"
    Organisation }o--|| OrganisationType : "is type"
    Organisation ||--o{ OrganisationAircraftType : "operates"
    Organisation ||--o{ OrganisationPersonPosition : "employs"
    Organisation ||--o{ Department : "has departments"
    
    OrganisationDetails }o--|| Country : "based in"
    OrganisationDetails }o--|| OrganisationType : "is type"
    OrganisationAircraftType }o--|| AircraftType : "aircraft"
    
    Organisation {
        int id PK
        int details_id FK
        boolean is_sanctioned_ofac
        datetime ofac_latest_update
    }
    
    OrganisationDetails {
        int id PK
        int organisation_id FK
        string registered_name
        string trading_name
        int country_id FK
        int type_id FK
        string tax_number
        boolean is_trading
        date trading_ceased_date
    }
    
    OrganisationType {
        int id PK
        string name
        boolean is_fuel_end_user
    }
    
    OrganisationSettings {
        int id PK
        int organisation_id FK
        string logo
        string color
        boolean is_brief_read_confirmation_needed
    }
```

## Airport Management System

```mermaid
erDiagram
    Organisation ||--o| AirportDetails : "is airport"
    AirportDetails ||--o{ AirportRunway : "has runways"
    AirportRunway }o--o{ AirportRunwayApproach : "has approaches"
    AirportRunway }o--o{ AirportRunwayLighting : "has lighting"
    AirportDetails }o--|| Region : "in region"
    AirportDetails }o--o{ AirportAtc : "has ATC"
    
    Organisation ||--o{ OrganisationAirportCategory : "assigns categories to"
    OrganisationAirportCategory }o--|| AirportCategory : "uses category"
    OrganisationAirportCategory }o--|| Organisation : "for airport"
    OrganisationAirportCategory }o--o| OrganisationAircraftType : "for aircraft type"
    
    AirportDetails {
        int id PK
        int organisation_id FK
        string icao_code UK
        string iata_code
        float latitude
        float longitude
        int elevation
        string timezone_name
        int maximum_weight
        string website_url
    }
    
    AirportCategory {
        int id PK
        string name "A, AA, A+, A2, B, BB, B+, B2, C, CC, C+, C2"
    }
    
    OrganisationAirportCategory {
        int id PK
        int organisation_id FK "Operator"
        int airport_id FK "Airport"
        int category_id FK
        int organisation_aircraft_type_id FK "NULL for All"
        unique(org_airport_aircraft)
    }
    
    AirportRunway {
        int id PK
        int airport_id FK
        string name
        float slope
        int length_m
        int width_m
        boolean is_active
    }
```

## Aircraft System

```mermaid
erDiagram
    AircraftType }o--|| AircraftDesignator : "has designator"
    AircraftType ||--o{ OrganisationAircraftType : "operated by"
    AircraftType ||--o{ PersonAircraftTypes : "person qualified"
    AircraftType ||--o{ BriefAircraftType : "in briefs"
    
    AircraftDesignator {
        int id PK
        string icao UK
        string iata
        string wtc "Wake Turbulence Category"
    }
    
    AircraftType {
        int id PK
        string manufacturer
        string model
        int type_designator_id FK
    }
```

## Briefing System

```mermaid
erDiagram
    Brief }o--|| Organisation : "for operator"
    Brief }o--|| Organisation : "at airport"
    Brief ||--o{ BriefSection : "has sections"
    Brief ||--o{ BriefElement : "contains elements"
    Brief ||--o{ BriefRevision : "has revisions"
    Brief ||--o{ BriefAircraftType : "for aircraft"
    Brief ||--o{ BriefAmendmentRecord : "has amendments"
    
    BriefElement }o--|| BriefElementSection : "in section"
    BriefElement }o--|| BriefElementType : "is type"
    BriefElement ||--o{ BriefSubElement : "has sub-elements"
    BriefElement ||--o{ BriefElementAttachment : "has attachments"
    
    BriefRequest }o--|| Organisation : "for airport"
    BriefRequest }o--|| User : "requested by"
    BriefRequest ||--o{ BriefRequestAircraftType : "for aircraft"
    
    Brief {
        int id PK
        string name
        string description
        string document
        string video
        date content_date
        string brief_type "pdf, generated, video, manual"
        boolean is_blanket
        boolean is_draft
        date valid_from_date
        date valid_to_date
        int operator_id FK
        int airport_id FK
    }
    
    BriefElement {
        int id PK
        int brief_id FK
        int section_id FK
        int type_id FK
        int sequence
        string slug
        string label
        text text
        text rich_text
        boolean is_active
    }
    
    BriefRequest {
        int id PK
        int airport_id FK
        int requested_by_id FK
        date brief_date
        text comments
        boolean is_done
    }
```

## Geographic Data Structure

```mermaid
erDiagram
    Continent ||--o{ Country : contains
    Country ||--o{ Region : "divided into"
    Country ||--o{ OrganisationDetails : "organisations in"
    Region ||--o{ AirportDetails : "airports in"
    
    Continent {
        string id PK
        string name
    }
    
    Country {
        int id PK
        string name
        string code UK
        int continent_id FK
        boolean in_eu
        boolean in_schengen
        boolean in_nato
    }
    
    Region {
        int id PK
        string name
        string code
        int country_id FK
    }
```

## User Authentication & Permissions Flow

```mermaid
flowchart TD
    A[User Login] --> B{Email Auth}
    B -->|Success| C[Load User Roles]
    C --> D{Check Permissions}
    D -->|Administrator| E[Full Access]
    D -->|Brief Writer| F[Brief Management]
    D -->|CMS Company Writer| G[Company Content]
    
    C --> H[Load Person]
    H --> I[Load Organisation Positions]
    I --> J[Organisation Context]
    
    subgraph "Permission Types"
        E
        F
        G
    end
```

## Data Flow for Airport Categories

```mermaid
sequenceDiagram
    participant U as User
    participant UI as Categories UI
    participant API as Django View
    participant DB as Database
    participant S2 as Select2
    
    U->>UI: Access Airport Categories
    UI->>API: Load Page
    API->>DB: Get Operator's Airports
    API->>DB: Get Aircraft Types
    API->>DB: Get Existing Categories
    DB-->>API: Data
    API-->>UI: Render Page
    
    UI->>S2: Initialize Select2
    U->>S2: Select New Airport
    S2->>UI: Airport Selected
    UI->>UI: Enable Category Cells
    
    U->>UI: Click Category Cell
    UI->>UI: Show Category Dropdown
    U->>UI: Select Category
    UI->>API: Save Category (AJAX)
    API->>DB: Update OrganisationAirportCategory
    DB-->>API: Success
    API-->>UI: Confirm Save
    UI->>UI: Update Display
    
    alt Excel Import
        U->>UI: Import Excel
        UI->>API: Upload File
        API->>API: Parse Excel
        API->>DB: Bulk Update Categories
    end
    
    alt Excel Export
        U->>UI: Export Excel
        UI->>API: Request Export
        API->>DB: Get All Categories
        API->>API: Generate Excel
        API-->>U: Download File
    end
```

## System Integration Points

```mermaid
graph LR
    subgraph "External Systems"
        OFAC[OFAC Sanctions API]
        Leon[Leon Flight Ops]
        S3[AWS S3 Storage]
        SES[AWS SES Email]
    end
    
    subgraph "SelfBrief CMS"
        Core[Core System]
        Briefs[Briefing System]
        Orgs[Organisations]
    end
    
    OFAC -->|Sanctions Check| Orgs
    Leon -->|Airport Categories| Orgs
    S3 -->|File Storage| Briefs
    SES -->|Notifications| Core
    
    subgraph "Background Tasks"
        Celery[Celery Workers]
        Redis[Redis Queue]
    end
    
    Core --> Redis
    Redis --> Celery
    Celery -->|Process| Briefs
    Celery -->|Reports| Orgs
```

## Database Schema Summary

### Table Prefixes
- **users_**: User authentication and connections
- **organisations_**: Organisation management
- **people_**: Person management
- **aircraft_**: Aircraft types and designators
- **briefs_**: Briefing documents and structure
- **core_**: System-wide entities (countries, regions)

### Key Relationships
1. **Multi-tenancy**: Organisations serve as the primary tenant boundary
2. **Polymorphic Relations**: Organisation can be Operator or Airport (type-based)
3. **Historical Tracking**: Details tables maintain change history
4. **Soft Deletes**: Trading ceased dates instead of deletion
5. **Audit Trail**: Created/Updated timestamps and user references

### Performance Considerations
- Indexed foreign keys for efficient joins
- Unique constraints on business keys
- Select_related and prefetch_related optimizations
- Custom managers for common queries
