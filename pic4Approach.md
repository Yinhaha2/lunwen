graph LR

%% 定义节点样式
classDef stepTitle fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,font-weight:bold;
classDef dataNode fill:#f9f9f9,stroke:#666,stroke-width:1px;
classDef filterNode fill:#fff3e0,stroke:#f57c00,stroke-width:1px,stroke-dasharray: 4 4;
classDef aiNode fill:#e8f5e9,stroke:#388e3c,stroke-width:2px;

%% --- Step 1 ---
subgraph Step1 [Step 1: Collecting agent-authored PRs]
    direction TB
    A[(AIDev Ecosystem Snapshot)]:::dataNode --> B[Extract PRs\n(ID, Agent, User)]:::dataNode
    B --> C[Fetch Artifacts\n(Commits, Diffs, Reviews, Issues)]:::dataNode
end

%% --- Step 2 ---
subgraph Step2 [Step 2: Filtering performance-related PRs]
    direction TB
    C --> D{Mutually Exclusive\nDetectors}:::filterNode
    D -->|LLM Classifier| E[llm_title_body_classifier\n(1,158 PRs)]:::dataNode
    D -->|Residual Tag| F[aidev_pr_task_type_perf_only\n(61 PRs)]:::dataNode
    E --> G[(Final Corpus\n1,219 PRs in 446 Repos)]:::stepTitle
    F --> G
end

%% --- Step 3 ---
subgraph Step3 [Step 3: Authoring gold JSON analyses]
    direction TB
    H[Human Annotation]:::dataNode --> I[Fixed JSON Schema\n& Analysis Protocol]:::dataNode
    H --> J[6 Gold Few-shot Exemplars]:::dataNode
end

%% --- Step 4 ---
subgraph Step4 [Step 4: Pipeline analysis with DeepSeek]
    direction TB
    G --> K[Assemble Bounded Payload\n(Truncated diffs & texts)]:::dataNode
    I -.->|Prompt Reference| L
    J -.->|Prompt Reference| L
    K --> L((DeepSeek-V4-Pro API)):::aiNode
    L --> M[/Structured JSON Labels\n(Layer, Antipattern, Outcome, etc.)/]:::stepTitle
end
