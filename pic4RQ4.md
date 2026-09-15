graph TD

%% 定义样式
classDef rootNode fill:#eceff1,stroke:#607d8b,stroke-width:2px,font-weight:bold;
classDef splitNode fill:#fff3e0,stroke:#f57c00,stroke-width:2px,stroke-dasharray: 5 5;
classDef fastNode fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,font-weight:bold;
classDef slowNode fill:#fce4ec,stroke:#c2185b,stroke-width:2px,font-weight:bold;
classDef featureBox fill:#f9f9f9,stroke:#999,stroke-width:1px;
classDef actionBox fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,stroke-dasharray: 4 4;
classDef warningBox fill:#ffebee,stroke:#d32f2f,stroke-width:2px;

%% 根节点
Root[Agent-Authored Performance PR]:::rootNode

%% 分流条件
Split{Stratification Dimensions:

Boundary Type, Review Depth, Speed}:::splitNode

Root --> Split

%% ---------------- 快路径 (Program A) ----------------
Split -->|Routine Stack &


Light Review| FastPath[Program A: The Fast Path]:::fastNode

FastPath --- FastFeatures[Characteristics:


• Median Lifetime: 0.075 hours


• Fast-merge Share: 78.8%


• Boundary: technical_stack (79.4% merge rate)]:::featureBox

FastFeatures --> FastActions[Interventions / Playbook A:


Keep patches atomic in routine stack


PR body: What changed & Why it is safe


Do not enforce 100-line rule


Avoid deep-review queue by default]:::actionBox

%% ---------------- 慢路径 (Program B) ----------------
Split -->|Evidence/Process Boundary &


Deep Review| SlowPath[Program B: The Slow Path]:::slowNode

SlowPath --- SlowFeatures[Characteristics:


• Median Lifetime: 23.9 hours


• Boundary: process (35.7%) / evidence_required (12.5%)


• Higher interaction & More numeric claims]:::featureBox

SlowFeatures --> SlowActions[Interventions / Playbook B:


Attach before/after table & repro steps


Treat CHANGES_REQUESTED as a core task


Declare risk early (runtime_vm, bundle-size)


Split evidence commit from code commit]:::actionBox

%% ---------------- 底部警示 ----------------
FastActions -.-> Warning[Finding 4 Takeaway:


Do NOT support adding a benchmark to EVERY performance PR.]:::warningBox
SlowActions -.-> Warning
