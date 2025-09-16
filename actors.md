# 🎭 Actor Catalog — Stencilit

This document defines the **actors** in the Stencilit system.  
Actors are categorized as **Human**, **Organization**, **System**, or **External**, making their role in the system crystal clear.  

---

## 👤 Human Actors
| Actor                     | Description                                                                 | Notes                                        |
|----------------------------|-----------------------------------------------------------------------------|---------------------------------------------|
| **Tattoo Artist (User)**   | Primary user; creates, reads, deletes stored signatures; manages designs, sessions, and client data. | Requires authentication & subscription |
| **Client**                 | Customer receiving a tattoo; represented as an entity in the system.        | Linked to tattoo sessions & metadata         |
| **Assistant**              | Supports tattoo artists in their workflow.                                  | Limited permissions compared to Tattoo Artist |
| **Studio Admin**           | Manages operations within a tattoo studio (artists, assistants, schedules). | Not a system-wide admin                      |
| **System Admin**           | Oversees overall system integrity and user accounts at the platform level.  | Separate from Studio Admin                   |

---

## 🏢 Organization Actors
| Actor            | Description                                                     | Notes                               |
|------------------|-----------------------------------------------------------------|-------------------------------------|
| **Tattoo Studio**| Business entity that employs tattoo artists and assistants; has a Studio Admin role. | Studio-level scope, not system-wide |

---

## ⚙️ System Actors
| Actor                     | Description                                                                 | Notes                                        |
|----------------------------|-----------------------------------------------------------------------------|---------------------------------------------|
| **Authentication Service** | Handles biometric and passcode authentication.                              | External / local hybrid                      |
| **Subscription Service**   | Verifies subscription status for access control.                            | Tied to App Store billing                    |
| **Database**               | PostgreSQL on server, SQLite on mobile; stores signatures, metadata, sync state. | Migration strategy in place                  |
| **Synchronization Service**| Syncs local SQLite data with remote PostgreSQL.                             | Ensures consistency                          |
| **Logging Service**        | Records key events and system errors.                                       | Centralized error handling planned           |

---

## 🌐 External Actors
| Actor                   | Description                                                                 | Notes                       |
|--------------------------|-----------------------------------------------------------------------------|-----------------------------|
| **App Store / Billing**  | Provides billing and subscription validation.                               | Apple / Google stores       |
| **Notification Service** | Sends push notifications to the user.                                       | Optional future enhancement |

---

## 📊 Actor Diagram

```mermaid
flowchart TB

  %% Human Actors
  subgraph Human["👤 Human Actors"]
    Artist["Tattoo Artist (User)"]
    Client
    Assistant
    StudioAdmin["Studio Admin"]
    SysAdmin["System Admin"]
  end

  %% Organization Actors
  subgraph Org["🏢 Organization Actors"]
    Studio["Tattoo Studio"]
  end

  %% System Actors
  subgraph System["⚙️ System Actors"]
    Auth["Authentication Service"]
    Sub["Subscription Service"]
    DB["Database (PostgreSQL/SQLite)"]
    Sync["Synchronization Service"]
    Log["Logging Service"]
  end

  %% External Actors
  subgraph External["🌐 External Actors"]
    Store["App Store / Billing"]
    Notify["Notification Service"]
  end

  %% Relationships
  Client --> Artist
  Assistant --> Artist
  Studio --> StudioAdmin
  Studio --> Artist
  Studio --> Assistant

  Artist --> DB
  StudioAdmin --> DB
  SysAdmin --> DB

  Artist --> Sync
  Sync --> DB

  Artist --> Auth
  Auth --> Sub
  Sub --> Store

  Log --> DB
  Notify --> Artist
