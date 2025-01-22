# Xtreamly Aave Loop Leverage Agent Demo

This Repo is WIP 

```mermaid
sequenceDiagram
    actor Web3_Data 
    participant XtreamlyAIModels as Xtreamly AI Models
    participant xtr_defi_backend as Xtr Defi Backend
    participant xtreamly_API as Xtreamly API
    participant ElizaAgentInstance
    participant AIModels as ChatGPT/Other AI Models
    participant On-chain as Blockchain

    loop Continual Data Ingestion
        Web3_Data->>XtreamlyAIModels: Ingest Data
        XtreamlyAIModels->>XtreamlyAIModels: Train and improve AI Models
    end

    loop Every 1 Minute
        xtr_defi_backend->>xtreamly_API: Request Volatility Prediction
        xtreamly_API->>XtreamlyAIModels: Fetch Prediction
        XtreamlyAIModels-->>xtreamly_API: Return Prediction
        xtreamly_API-->>xtr_defi_backend: JSON Prediction

        xtr_defi_backend->>xtr_defi_backend: Process JSON & Create proposedTradeAction
        xtr_defi_backend->>ElizaAgentInstance: Send proposedTradeAction
        ElizaAgentInstance->>AIModels: Evaluate proposedTradeAction
        AIModels-->>ElizaAgentInstance: Return Decision
        ElizaAgentInstance-->>xtr_defi_backend: Forward Decision

        xtr_defi_backend->>xtr_defi_backend: Decide & Prepare Transaction
        xtr_defi_backend->>On-chain: Execute Transaction (Aave v3, Uni v3/v4 ...)
        On-chain->>xtr_defi_backend: Return Transaction Receipt
        xtr_defi_backend->>xtr_defi_backend: Verify Success/Failure
    end

```



```mermaid
flowchart TD
    A[Start]

    subgraph Data_Ingestion [Xtreamly DeFi AI Models]
        direction TB
        B[Ingest Web3 Data] --> C[Improve AI Models]
        C --> B
    end

    A --> Data_Ingestion
    Data_Ingestion --> D[xtr_defi_backend]

    subgraph xtr_defi_backend [xtr_defi_backend]
        direction TB
        D --> E[Request Prediction]
        E --> F[Process Prediction]
        F --> G[Evalute Prediction in Policy]
        G --> I[Process & Prepare Action]
        I --> J[Send to ElizaAgent]
    end

    subgraph ElizaAgent_AI [ElizaOS Agents & AI Models]
        direction TB
        
        J --> K[Fetch Live Market Data]
        K --> L[Eval sugg. Action w/ AI Models]
        L --> M[await Suggestion]
        M --> N[Forward to Backend]
    end

    subgraph xtr_defi_backend [xtr_defi_backend]
        direction TB
        N --> R{TradeAction Decision?}
        R --> |Yes| O[Prepare Transaction]
        R --> |No| E
        O --> P[Execute on-chain]
        P --> Q[Get Receipt]
        Q --> S{Verify Outcome}
    end

    S --> |Successful| E
    S --> |Unsuccessful @3 attempts| O
```

## Quick Start (WIP)

1)
```bash
git clone git@github.com:Xtreamly-Team/looptrader_agent_DEMO.git
```

2)

