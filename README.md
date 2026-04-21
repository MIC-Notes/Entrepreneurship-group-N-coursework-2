# Entrepreneurship | Group N: Coursework 2

This repository contains the final submission assets for **Coursework 2**. It includes our pitch materials, technical documentation, and legal frameworks developed throughout the module.

---

## Submission Deliverables

| Asset | Source / Link |
| :--- | :--- |
| **Pitch Deck** | [Link to Presentation](https://docs.google.com/presentation/d/12Wf0biEPcASuq50PFNK5GeybYlofFMy-/edit?usp=sharing&ouid=102049752064715129815&rtpof=true&sd=true) |
| **Demo Video** | TODO SAM |
| **Terms of Service:** | [Link](TODO_BRIAN)
| **Ethics & Legal Documentation:** | [Link](TODO_BRIAN)
| **Visual Artefacts** | [View Artefacts](./posters) |

---

## Technical Documentation

### Final Product
The core mobile application developed for this project:
**Repository:** [MIC-Notes Mobile](https://github.com/MIC-Notes/mic-mobile)
**Architecture Diagram:**
  
```mermaid
graph TD
    %% Define styles
    classDef ui fill:#E1F5FE,stroke:#0288D1,stroke-width:2px;
    classDef state fill:#E8F5E9,stroke:#388E3C,stroke-width:2px;
    classDef service fill:#FFF3E0,stroke:#F57C00,stroke-width:2px;
    classDef data fill:#ECEFF1,stroke:#607D8B,stroke-width:2px;

    subgraph Presentation_Layer ["Presentation Layer (UI Widgets)"]
        AppShell["AppShell<br>(Bottom Navigation)"]:::ui
        HomeScreen["HomeScreen<br>(Pomodoro Timer)"]:::ui
        LecturesScreen["LecturesScreen<br>(Library)"]:::ui
        AnalysisScreen["AnalysisScreen<br>(Dashboard)"]:::ui
        AiAssistantScreen["AiAssistantScreen<br>(MIC Chat)"]:::ui
        PanoptoBrowser["PanoptoBrowserScreen<br>(WebView Scraper)"]:::ui
        ReaderScreen["ReaderScreen<br>(Transcript Viewer)"]:::ui

        AppShell --> HomeScreen
        AppShell --> LecturesScreen
        AppShell --> AnalysisScreen
        
        LecturesScreen --> PanoptoBrowser
        LecturesScreen --> ReaderScreen
        HomeScreen -.->|"Opens"| AiAssistantScreen
    end

    subgraph State_Management ["State Management Layer (InheritedNotifier)"]
        PomodoroState["PomodoroState<br>(Timer Logic & Phases)"]:::state
        AiState["AiState<br>(RAG Flow & Chat History)"]:::state
    end

    subgraph Service_Layer ["Service Layer (Business Logic)"]
        ApiService["ApiService<br>(Mock Settings API)"]:::service
        PanoptoService["PanoptoService<br>(JS Injection & Extraction)"]:::service
        GemmaService["GemmaService<br>(Local LLM & Embeddings)"]:::service
        RagService["RagService<br>(Chunking & Vector Math)"]:::service
    end

    subgraph Data_Storage ["Data & Storage Layer"]
        ObjectBox[("ObjectBox<br>(Vector DB / TranscriptChunk)")]:::data
        FileSystem[("Local File System<br>(.txt files)")]:::data
    end

    %% UI to State Connections
    HomeScreen -->|"Listens to & Updates"| PomodoroState
    AiAssistantScreen -->|"Listens to & Prompts"| AiState
    LecturesScreen -->|"Triggers ingestion"| AiState

    %% UI to Service Connection (Bypassing State for Web Scraper)
    LecturesScreen -->|"Opens & Listens to"| PanoptoService

    %% State to Service Connections
    PomodoroState -->|"Fetches/Saves"| ApiService
    AiState -->|"Streams Generation"| GemmaService
    AiState -->|"Retrieves Context"| RagService
    
    %% Service to Service Connections
    RagService -->|"Requests Float32 Embeddings"| GemmaService

    %% Service to Data Connections
    RagService -->|"Queries & Stores Vectors"| ObjectBox
    LecturesScreen -->|"Saves raw transcript"| FileSystem
    ReaderScreen -->|"Reads raw transcript"| FileSystem
```
