# 📖 Naming Conventions & Style Guide

This document defines naming conventions across the **Stencilit** system.  
It ensures consistency between databases, domain models, APIs, and UI layers.

---

## 1. Database (PostgreSQL / SQLite)
- **Convention**: `snake_case`
- **Examples**:  
  - `client_id`  
  - `is_registered`  
  - `created_at`  
- **Reason**: SQL style guides favor `snake_case` for readability.

---

## 2. Domain Models (Core Entities)
- **Convention**: `camelCase`
- **Examples**:  
  - `clientId`  
  - `isRegistered`  
  - `createdAt`  
- **Reason**: Idiomatic in Swift, Kotlin, TypeScript, etc.

---

## 3. API Contracts (JSON / REST / GraphQL)
- **Convention**: `camelCase`
- **Examples**:  
  ```json
  {
    "clientId": "1234",
    "isRegistered": true,
    "createdAt": "2025-09-16T12:00:00Z"
  }
  ```
- **Notes**:  
  - Matches domain models for simplicity.  
  - External APIs may differ — map at integration boundaries.

---

## 4. UI Layer
- **Convention**: `camelCase`
- **Examples**:  
  - `clientName`  
  - `sessionDate`  
- **Reason**: Matches domain models, reduces friction between layers.

---

## 5. Mapping Rules
- **Database → Repository**: snake_case → camelCase  
- **Repository → Domain Models**: keep camelCase  
- **Domain Models → API (JSON)**: camelCase via serialization tools  
- **Domain → UI**: direct pass-through (camelCase)

---

## 6. Enforcement
- **Database**: migrations + SQLFluff (linting)  
- **Domain / UI**: code reviews + IDE conventions  
- **API**: OpenAPI/Swagger schemas define JSON shape  
- **Serialization**:  
  - Swift → `CodingKeys`  
  - Kotlin → `@SerializedName`  
  - TypeScript → custom mappers if needed

---

## 7. Philosophy
- **snake_case in storage**  
- **camelCase in code**  
- **align domain models and JSON** to avoid surprises

---
