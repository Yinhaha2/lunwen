flowchart TB
  subgraph COLS["Fig. 1  Background  ·  same speech act, three endings"]
    direction LR

    subgraph A["(a) Fast path"]
      direction TB
      A0["MontrealAI / AGI-Alpha-Agent-v0  PR 1377\nOpenAI Codex  ·  28 lines"]
      A1["CLAIM\nSpeed up Pareto front with a single scan"]
      A2["EVIDENCE\nnarrative only\nno table  ·  no numbers  ·  no review"]
      A3["OUTCOME  MERGED in 7 seconds\ntechnical_stack  ·  fast_merge"]
      A0 --> A1 --> A2 --> A3
    end

    subgraph B["(b) Evidence cliff"]
      direction TB
      B0["vercel / turborepo  PR 10623\nCursor  ·  230 lines"]
      B1["CLAIM\nStream 8KB reads to hash large files\nfaster and with less memory"]
      B2["EVIDENCE\nPERFORMANCE_IMPROVEMENT_SUMMARY.md\nmaintainer: still not a real benchmark\nCHANGES_REQUESTED: BufReader + numbers"]
      B3["OUTCOME  CLOSED after 5.6 days\nevidence_required  ·  missing_benchmark"]
      B0 --> B1 --> B2 --> B3
    end

    subgraph C["(c) Process drop-off"]
      direction TB
      C0["Cap-go / capgo  PR 1065\nDevin  ·  77 lines"]
      C1["CLAIM\nAND search + lazy MAU load\n+ 800ms debounce for large lists"]
      C2["EVIDENCE\nnarrative only\nbot comments  ·  zero human review"]
      C3["OUTCOME  CLOSED in 11.5 hours\nprocess  ·  no maintainer sentence"]
      C0 --> C1 --> C2 --> C3
    end
  end
