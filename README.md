# 📸 Android Photo Frame: Enterprise Ecosystem & Gamification Engine

![Kotlin](https://img.shields.io/badge/Kotlin-1.9.0-purple?style=for-the-badge&logo=kotlin)
![Compose](https://img.shields.io/badge/Compose-Material3-blue?style=for-the-badge&logo=android)
![Firebase](https://img.shields.io/badge/Firebase-Firestore%20%7C%20Storage-orange?style=for-the-badge&logo=firebase)
![Architecture](https://img.shields.io/badge/Architecture-Clean%20%2B%20MVVM-green?style=for-the-badge)
![Monetization](https://img.shields.io/badge/Monetization-Hybrid-red?style=for-the-badge)

---

## 📖 Project Overview

**Android Photo Frame** — це професійний Android-застосунок з вертикальною орієнтацією, розроблений для художнього оформлення фотографій. Продукт поєднує в собі потужний редактор колажів із глибокою RPG-системою прогресії та багаторівневою гібридною монетизацією.

🎯 **Основна мета:** перетворити рутинний процес редагування фото на захопливу гру з винагородами.

---

## 🛠 Technical Stack (Senior Level)

| Category | Technologies |
|---|---|
| Language | Kotlin (Coroutines + Flow) |
| UI Framework | Jetpack Compose (Modern declarative UI, Material 3) |
| Architecture | MVVM + Clean Architecture |
| Image Processing | Custom Canvas API + Glide |
| Backend | Firebase (Firestore — прогрес, Storage — асети, Auth) |
| Monetization | AdMob SDK + Google Play Billing Library |
| DI | Hilt / Koin |

---

## 🧠 Core Product Logic & Screen Flow

### 3.1 Navigation Structure

Система навігації побудована на чіткому розділенні етапів редагування та перегляду:

- 🏠 **Home Screen:** Вибір типу колажу (Solo, Couple, Collage)
- 🖼️ **Frame Selector:** Скрол-інтерфейс для вибору рамки (Premium/Free)
- 📂 **Image Picker:** Інтеграція з галереєю для вибору фото
- 🎨 **Editor Space:** Робоча область (gestures, crop, filter)
- 📤 **Share Screen:** Генерація лінку та шеринг результату
- 🏛️ **Gallery:** Архів завершених робіт

---

### 3.2 System Architecture Diagrams

#### 🏗 Component Diagram (MVVM + Clean Architecture)

```mermaid
graph TD
    subgraph Presentation_Layer["Presentation Layer"]
        UI[Compose Screens] --> VM[ViewModel]
        VM --> UI_State[State Updates]
        User[User Events] --> UI
    end

    subgraph Domain_Layer["Domain Layer"]
        VM --> UC[Use Cases]
        UC --> Repo_Int[Repository Interfaces]
        UC -.->|Business Logic| Models[Domain Models]
    end

    subgraph Data_Layer["Data Layer"]
        Repo_Int -.-> Repo_Impl[Repository Implementation]
        Repo_Impl --> Local[Room DB / SharedPrefs]
        Repo_Impl --> Remote[Firebase / REST API]
    end

    style Presentation_Layer fill:#e1f5fe,stroke:#01579b
    style Domain_Layer fill:#fff3e0,stroke:#ff6f00
    style Data_Layer fill:#f3e5f5,stroke:#4a148c
```

---

## 🔄 Data Flow Diagram (Image Processing Pipeline)

```mermaid
flowchart LR
    Source["📷 Image Sources"] --> Selector["🖼️ Frame Selector"]
    Selector --> Engine["🎨 Editor Engine"]

    subgraph Engine_Process["Processing"]
        Canvas[Canvas API]
        Matrix[Matrix Ops]
        Filters[Filters]
    end

    Engine --> Engine_Process

    Engine_Process --> Render["💾 Render & Save"]
    Render --> Metadata["📊 Metadata Extractor"]
    Metadata --> Output["🏆 Gamified Output"]

    subgraph Output_Result["Result"]
        XP[+XP Gain]
        Stars[+Star Reward]
        Disk[Save to Disk]
    end

    Output --> Output_Result
```

---

## ⏱️ Sequence Diagram (User Creates & Saves Frame)

```mermaid
sequenceDiagram
    actor User
    participant UI as UI Layer
    participant VM as ViewModel
    participant Repo as Repository
    participant Cloud as Firebase

    User->>UI: Select Frame
    UI->>VM: loadFrame()
    VM->>Repo: fetchFrame()
    Repo->>Cloud: getFrameData()
    Cloud-->>Repo: Frame JSON/Asset
    Repo-->>VM: Frame Domain Model
    VM-->>UI: Update State (Frame Loaded)

    User->>UI: Choose Photo & Edit
    UI->>VM: processImage(filters, crop)

    User->>UI: Click "Save"
    UI->>VM: saveResult()
    VM->>Repo: saveToDB() & upload()
    Repo->>Cloud: Write Metadata
    Cloud-->>Repo: Success

    par Gamification
        VM->>Repo: grantXP(amount)
        Repo->>Cloud: updateLevel()
    and UI Update
        VM-->>UI: Show Success & XP Animation
    end
```

---
## 3.3 Database Schema (Firestore)

```mermaid
erDiagram
    USER ||--o{ CREATION : creates
    USER {
        string userId PK
        int level
        int xp
        int stars
        string-array unlockedFrames
        int dailyStreak
    }

    CREATION {
        string creationId PK
        string frameId FK
        string-array imageUrls
        string-array filtersApplied
        timestamp createdAt
        int xpEarned
    }

    FRAME {
        string frameId PK
        string category
        boolean isPremium
        int unlockCost
        string downloadUrl
    }

    QUEST ||--|{ USER : assigned_to
    QUEST {
        string questId PK
        string type
        int xpReward
        int starReward
        json requirements
    }
```   map requirements
    }
---

## 3.4 Editing Capabilities

- ✅ Drag-and-drop  
- ✅ Pinch-to-zoom  
- ✅ Smart Swap (Long Press)  
- ✅ Filter Engine: Brightness, Contrast, Retro, B&W, Cinema  

---

## 🎮 Gamification: The Retention Engine

### 4.1 Experience (XP) System

Прогресія рівнів (макс. **50**). Кожен наступний рівень вимагає на **200 XP** більше.

- **+10 XP** — Створення рамки  
- **+05 XP** — Використання фільтрів  
- **+20 XP** — Шерінг у соцмережах  
- **+50 XP** — Виконання простого квесту  

---

### 4.2 Star Economy (Soft Currency) ⭐

Зірки — валюта для розблокування контенту.

- **Daily Bonus:** 7-денний цикл  
- **Level Up:** +10–25 ⭐  
- **Ads:** +10 ⭐ за Rewarded Video  

---

### 4.3 Daily Bonus & Quests

- **Bonus:** При пропуску дня бонус не скидається (нагорода за "вчора")  
- **Quests:** Щоденні ("Зроби 2 фото") та тематичні ("Альбом подорожі")  

---

## 💰 Monetization Framework (Hybrid Model)

| Method | Logic | UX Impact |
|------|------|-----------|
| Rewarded Ads | 20-сек відео за зірки або преміум-рамку | ✅ User-initiated (Friendly) |
| Interstitials | 5-сек ролики при запуску (чергуються з тріалом) | ⚠️ Обмежена частота |
| Subscription | 4.99 грн/тиждень — No Ads + High-Res | 💎 3-денний Trial |
| Premium Frames | Тимчасовий доступ за 2 рекламних ролики | 🔒 Візуальне маркування |

---

## 🚀 Development Roadmap

**Phase 1 (MVP)**  
Логіка вибору рамок, завантаження фото, drag/zoom, збереження  

**Phase 2 (UX)**  
Полірування UI (Animations), інтеграція фільтрів  

**Phase 3 (Meta)**  
Гейміфікація (XP, рівні), квести, AdMob & Billing  

> **Note:** Реклама ніколи не показується під час активного процесу редагування, щоб не псувати User Experience.

---
