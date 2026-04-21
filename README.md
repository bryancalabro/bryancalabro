# Engineering experiences for people who like their software smart, useful, and a little dangerous.

![AI Orchestration](https://img.shields.io/badge/AI%20Orchestration-1a1a2e?style=flat-square&logoColor=white) ![Agent Architecture](https://img.shields.io/badge/Agent%20Architecture-0f3460?style=flat-square&logoColor=white) ![Delivery Pipelines](https://img.shields.io/badge/Delivery%20Pipelines-0d2e1a?style=flat-square&logoColor=white)

---

**Recent apps**

- 🤖 <a href="https://agent.calabrodesign.com/" target="_blank" rel="noopener noreferrer">Agentic AI</a>
- 🎨 <a href="https://css.calabrodesign.com/" target="_blank" rel="noopener noreferrer">CSS Extractor</a>
- 📄 <a href="https://parser.calabrodesign.com/" target="_blank" rel="noopener noreferrer">Document Parser</a>
- 🍪 <a href="https://crumbs.calabrodesign.com/" target="_blank" rel="noopener noreferrer">CrumbTrail</a>

---

## Architectures & Frameworks

<details>
<summary><b>1. Three Phases Maturity Model</b><br><i>Mapping the maturity phases from human-driven orchestration to fully autonomous agentic systems.</i></summary>
<br>

```mermaid
flowchart LR
    subgraph P1["PHASE 1 — Human-Driven"]
        direction TB
        P1A[Human as Orchestrator\nPrompts · Directs · Decides]
        P1B[LLM as Tool\nOn-Demand · Single Task · No Memory]
        P1C[Output Quality\nDepends on Prompt Skill]
        P1D[Speed Gain\nReal but Inconsistent]
        P1E[Bottleneck\nHuman Availability]
        P1A --> P1B --> P1C --> P1D --> P1E
    end

    P1 ==>|automate sequences| P2

    subgraph P2["PHASE 2 — Agent-Assisted"]
        direction TB
        P2A[Agent as Executor\nHandles Task Sequences]
        P2B[Human at Checkpoints\nReview · Approve · Intervene]
        P2C[Skill Files Loaded\nSpecialized Role Behavior]
        P2D[Output Quality\nShaped by Evals + Human Review]
        P2E[Bottleneck\nHandoff Clarity · Context Quality]
        P2A --> P2B --> P2C --> P2D --> P2E
    end

    P2 ==>|close the loop| P3

    subgraph P3["PHASE 3 — Fully Autonomous"]
        direction TB
        P3A[Agent as Orchestrator\nPlans · Delegates · Self-Corrects]
        P3B[Human at Command Layer\nMonitor · Set Goals · Override]
        P3C[Feedback Loops Active\nSystem Improves Continuously]
        P3D[Output Quality\nSystematic · Validated · Consistent]
        P3E[Bottleneck\nGoal Clarity · Trust Calibration]
        P3A --> P3B --> P3C --> P3D --> P3E
    end

    subgraph NOW["WHERE MOST TEAMS ARE TODAY"]
        POS[Somewhere Between\nPhase 1 and Phase 2]
    end

    P1 -.->|most teams| NOW
    P2 -.->|leading teams| NOW

    classDef p1Style fill:#2e0a0a,stroke:#e74c3c,color:#ffcccc
    classDef p2Style fill:#2e1f00,stroke:#f5a623,color:#ffe0a0
    classDef p3Style fill:#0d2e1a,stroke:#2ecc71,color:#e0e0ff
    classDef nowStyle fill:#16213e,stroke:#7ec8e3,color:#e0e0ff,stroke-dasharray:5 5

    class P1A,P1B,P1C,P1D,P1E p1Style
    class P2A,P2B,P2C,P2D,P2E p2Style
    class P3A,P3B,P3C,P3D,P3E p3Style
    class POS nowStyle
```

</details>

<details>
<summary><b>2. SDLC Inversion</b><br><i>The paradigm shift from traditional document-driven SDLC to AI-augmented system-driven SDLC.</i></summary>
<br>

```mermaid
flowchart LR
    subgraph BEFORE["TRADITIONAL SDLC — Documents Drive Development"]
        direction TB
        B1[Requirements Written\nBy Humans · Upfront · Manual]
        B2[Architecture Designed\nFrom Specs · Whiteboard · Docs]
        B3[Development Begins\nHumans Execute · Slow · Linear]
        B4[Testing & QA\nManual · Late · Reactive]
        B5[Documentation\nWritten After · Incomplete · Stale]
        B6[Delivery\nLate · Over Budget · Partial]
        B1 --> B2 --> B3 --> B4 --> B5 --> B6
    end

    BEFORE ==>|inversion| AFTER

    subgraph AFTER["AI-AUGMENTED SDLC — Systems Drive Documentation"]
        direction TB
        A1[Prototype First\nAgents Scaffold · Fast · Iterative]
        A2[Requirements Derived\nFrom Working System · Auto-Generated]
        A3[Architecture Emerges\nAgents Design · ADRs · Diagrams]
        A4[Testing Continuous\nAgents Generate · Automated · Proactive]
        A5[Docs as Output\nLive · Accurate · Agent-Maintained]
        A6[Delivery\nCompressed · Iterative · Validated]
        A1 --> A2 --> A3 --> A4 --> A5 --> A6
    end

    classDef beforeStyle fill:#2e0a0a,stroke:#e74c3c,color:#ffcccc
    classDef afterStyle fill:#0d2e1a,stroke:#2ecc71,color:#e0e0ff

    class B1,B2,B3,B4,B5,B6 beforeStyle
    class A1,A2,A3,A4,A5,A6 afterStyle
```

</details>

<details>
<summary><b>3. Stack Layer Diagram</b><br><i>The foundational architecture stack mapping models, skills, agents, orchestration, and surface delivery.</i></summary>
<br>

```mermaid
flowchart TD
    subgraph L1["⑤ DELIVERY SURFACE — Where It Becomes Real"]
        API[REST API\nExternal Consumers]
        DASH[Internal Dashboard\nOps · Review · Command]
        BATCH[Scheduled Jobs\nBatch Export · CI Triggers]
    end

    subgraph L2["④ ORCHESTRATION — The Brain"]
        ROUTER[Task Router\nWhich Agent · What Order · What Context]
        RETRY[Retry & Error Policy\nFailure Handling · Fallback Routes]
        HANDOFF[Agent Handoff\nOutput Envelope · State Transfer]
        ROUTER --> HANDOFF
        ROUTER --> RETRY
        RETRY --> ROUTER
    end

    subgraph L3["③ AGENTIC LAYER — Where Planning Happens"]
        PLANNER[Planner\nDecompose · Sequence · Decide]
        EXECUTOR[Executor\nCall Tools · Run Skills · Produce Output]
        VALIDATOR[Validator\nCheck Output · Score · Accept or Retry]
        PLANNER --> EXECUTOR --> VALIDATOR
        VALIDATOR -->|retry| PLANNER
    end

    subgraph L4["② SKILL FILES — How Agents Behave"]
        REQ[Requirements Agent\nreqs-agent.md]
        ARCH[Architecture Agent\narch-agent.md]
        CODE[Code Gen Agent\ncodegen-agent.md]
        QA[QA Agent\nqa-agent.md]
    end

    subgraph L5["① FOUNDATION — Models, Retrieval & Tools"]
        LLM[Foundation Models\nReasoning · Generation · Evaluation]
        VEC[Vector Index\nRAG · Semantic Search]
        RULES[Rule Engine\nDeterministic Logic · Validation]
        APIS[External APIs\nData Sources · Services]
    end

    L5 -->|context + reasoning| L4
    L4 -->|loaded as system context| L3
    L3 -->|planned tasks + output| L2
    L2 -->|validated results| L1

    classDef l1Style fill:#0d2e1a,stroke:#2ecc71,color:#e0e0ff
    classDef l2Style fill:#0d2137,stroke:#7ec8e3,color:#e0e0ff
    classDef l3Style fill:#0f3460,stroke:#7ec8e3,color:#e0e0ff
    classDef l4Style fill:#1a0a2e,stroke:#9b59b6,color:#e0e0ff
    classDef l5Style fill:#1a1a2e,stroke:#4a9eff,color:#e0e0ff

    class API,DASH,BATCH l1Style
    class ROUTER,RETRY,HANDOFF l2Style
    class PLANNER,EXECUTOR,VALIDATOR l3Style
    class REQ,ARCH,CODE,QA l4Style
    class LLM,VEC,RULES,APIS l5Style
```

</details>

<details>
<summary><b>4. Ticket Lifecycle & Human Revision Loop</b><br><i>The full round-trip of a ticket — from creation in an external tracker, through the AI pipeline, back to the tracker for human review, and re-entry on edit request.</i></summary>
<br>

```mermaid
flowchart TD
    subgraph TRACKER["① TRACKER — Ticket Created"]
        TK[Ticket Opened\nJira · Linear · GitHub Issues]
        TF[Fields\nTitle · Description · Acceptance Criteria · Labels · Epic]
        TK --> TF
    end

    subgraph TRIGGER["② TRIGGER — Pipeline Activated"]
        WH[Webhook / Scheduled Poll\nDetects New or Updated Ticket]
        NRM[Normalize & Ingest\nExtract Metadata · Link Artifacts]
        TF --> WH --> NRM
    end

    subgraph PIPELINE["③ AI PIPELINE — See Diagram 5 for Full Detail"]
        CTX[Context Assembly\nRetrieve · Pack · Validate]
        ORCH[Orchestrate & Execute\nRequirements · Architecture · Code Gen · QA]
        ENV[Output Envelope\nPR · Updated Ticket Body · ADR · Artifacts]
        NRM --> CTX --> ORCH --> ENV
    end

    subgraph PUSHBACK["④ PUSH BACK — Update the Tracker"]
        UPD[Agent Writes Back\nUpdated Description · Linked PR · Comment with Artifacts]
        STAT[Status Updated\nIn Review · Awaiting Approval]
        ENV --> UPD --> STAT
    end

    subgraph REVIEW["⑤ HUMAN REVIEW — Review in Tracker"]
        HR{Human Reviews Ticket\nApprove · Request Edits · Reject}
        STAT --> HR
    end

    subgraph EDITLOOP["⑥ EDIT REQUEST — Re-enters the Pipeline"]
        EC[Revision Comment Added\nHuman Describes Requested Changes]
        WH2[Webhook Fires Again\nRevision Context Appended to Ticket]
        RELOAD[Pipeline Re-runs\nPrior Output + Edit Instructions Loaded as Context]
        EC --> WH2 --> RELOAD
    end

    subgraph RESOLVE["⑦ RESOLVE — Done"]
        APPROVE[Approved\nTicket Closed · PR Merged]
        REJECT[Rejected\nTicket Moved to Backlog · Archived]
        HR -->|approved| APPROVE
        HR -->|rejected| REJECT
    end

    HR -->|request edits| EC
    RELOAD -.->|re-enters pipeline| CTX

    classDef trackerStyle fill:#1a1a2e,stroke:#4a9eff,color:#e0e0ff
    classDef triggerStyle fill:#16213e,stroke:#4a9eff,color:#e0e0ff
    classDef pipelineStyle fill:#0f3460,stroke:#7ec8e3,color:#e0e0ff
    classDef pushbackStyle fill:#0d2e1a,stroke:#2ecc71,color:#e0e0ff
    classDef humanStyle fill:#2e1f00,stroke:#f5a623,color:#ffe0a0,stroke-width:2px
    classDef editStyle fill:#2e1f00,stroke:#f5a623,color:#ffe0a0
    classDef resolveStyle fill:#0d2e1a,stroke:#2ecc71,color:#e0e0ff
    classDef rejectStyle fill:#2e0a0a,stroke:#e74c3c,color:#ffcccc

    class TK,TF trackerStyle
    class WH,NRM triggerStyle
    class CTX,ORCH,ENV pipelineStyle
    class UPD,STAT pushbackStyle
    class HR humanStyle
    class EC,WH2,RELOAD editStyle
    class APPROVE resolveStyle
    class REJECT rejectStyle
```

</details>

<details>
<summary><b>5. AI Delivery Pipeline</b><br><i>The end-to-end workflow of how tickets and tasks move through the delivery pipeline.</i></summary>
<br>

```mermaid
flowchart TD
    subgraph INTAKE["① INTAKE — Discover & Scope"]
        JT[Ticket / Epic\nUser Story · Acceptance Criteria]
        LD[Legacy Artifacts\nDocs · Specs · Business Rules]
        JT -->|linked artifacts| LD
    end

    subgraph INGEST["② INGEST — Normalize"]
        OCR[Document Parser\nExtract → Markdown]
        IDX[Index & Tag\nModule · Process · Entity]
        LD --> OCR --> IDX
        JT -->|ticket context| IDX
    end

    subgraph CONTEXT["③ CONTEXT — Retrieve & Pack"]
        RAG[RAG Retrieval\nQuery by Ticket / Component]
        CTX[Context Pack\nRequirements · Constraints · Schema]
        IDX --> RAG --> CTX
    end

    subgraph ORCHESTRATE["④ ORCHESTRATE — Plan"]
        PLANNER[Requirements Agent\nDecompose · Map · Prioritize]
        HC1{Human Checkpoint\nApprove Requirements}
        ARCH[Architecture Agent\nDesign · ADRs · Diagrams]
        HC2{Human Checkpoint\nApprove Architecture}
        CTX --> PLANNER --> HC1
        HC1 -->|approved| ARCH
        HC1 -->|revise| PLANNER
        ARCH --> HC2
        HC2 -->|revise| ARCH
    end

    subgraph EXECUTE["⑤ EXECUTE — Build"]
        CODEGEN[Code Gen Agent\nScaffold · Implement · Refactor]
        TESTGEN[Test Gen Agent\nUnit · Integration · Contract]
        HC2 -->|approved| CODEGEN
        CODEGEN --> TESTGEN
    end

    subgraph VALIDATE["⑥ VALIDATE — Review"]
        EVAL[Eval Layer\nLLM-as-Judge · Rule Checks]
        QA[QA Agent\nCoverage · Regression · Accessibility]
        HC3{Human Checkpoint\nApprove Release}
        TESTGEN --> EVAL --> QA --> HC3
        HC3 -->|revise| CODEGEN
    end

    subgraph DELIVER["⑦ DELIVER — Ship"]
        PR[Pull Request\nCode · Tests · Docs]
        DEPLOY[CI/CD Pipeline\nBuild · Deploy · Release]
        HC3 -->|approved| PR --> DEPLOY
    end

    subgraph OBSERVE["⑧ OBSERVE — Learn"]
        MON[Monitor\nDrift · Failures · Edge Cases]
        IMPROVE[Improve\nFeed back → Prompts · Index · Evals]
        DEPLOY --> MON --> IMPROVE
        IMPROVE -->|refine retrieval| RAG
        IMPROVE -->|update scope| JT
    end

    classDef intakeStyle fill:#1a1a2e,stroke:#4a9eff,color:#e0e0ff
    classDef ingestStyle fill:#16213e,stroke:#4a9eff,color:#e0e0ff
    classDef contextStyle fill:#0f3460,stroke:#7ec8e3,color:#e0e0ff
    classDef orchStyle fill:#0d2137,stroke:#7ec8e3,color:#e0e0ff
    classDef execStyle fill:#1a0a2e,stroke:#9b59b6,color:#e0e0ff
    classDef valStyle fill:#2e0d1a,stroke:#e74c8b,color:#e0e0ff
    classDef delivStyle fill:#0d2e1a,stroke:#2ecc71,color:#e0e0ff
    classDef obsStyle fill:#2e2a0d,stroke:#f39c12,color:#e0e0ff
    classDef humanStyle fill:#2e1f00,stroke:#f5a623,color:#ffe0a0,stroke-width:2px

    class JT,LD intakeStyle
    class OCR,IDX ingestStyle
    class RAG,CTX contextStyle
    class PLANNER,ARCH orchStyle
    class CODEGEN,TESTGEN execStyle
    class EVAL,QA valStyle
    class PR,DEPLOY delivStyle
    class MON,IMPROVE obsStyle
    class HC1,HC2,HC3 humanStyle
```

</details>

<details>
<summary><b>6. Context Assembly Flow</b><br><i>The process of extracting, indexing, and assembling unstructured tickets into agent-ready context packs.</i></summary>
<br>

```mermaid
flowchart TD
    subgraph TICKET["① TICKET — Extract Metadata"]
        TK[Incoming Ticket]
        TM[Metadata Extraction\nEpic · Component · Labels]
        AC[Acceptance Criteria\nConstraints · Definition of Done]
        LK[Linked Artifacts\nDocs · Parent Epics · Dependencies]
        TK --> TM
        TK --> AC
        TK --> LK
    end

    subgraph INDEX["② INDEX — Query the Document Store"]
        QRY[Index Query\nMatch by Component · Entity · Process]
        HIT{Documents Found?}
        MATCH[Matched Artifacts\nSpecs · Business Rules · Data Models]
        FLAG[⚠ Scope Gap Flagged\nNo Match → Undiscovered Scope]
        LK --> QRY
        TM --> QRY
        QRY --> HIT
        HIT -->|yes| MATCH
        HIT -->|no| FLAG
        FLAG -->|create ticket| TK
    end

    subgraph ASSEMBLE["③ ASSEMBLE — Pack the Context"]
        RANK[Rank & Filter\nRelevance · Recency · Token Budget]
        CHUNK[Chunk & Structure\nRequirements · Constraints · Schema]
        CTX[Context Pack\nReady for Agent Consumption]
        MATCH --> RANK --> CHUNK --> CTX
        AC --> CHUNK
    end

    subgraph GATE["④ GATE — Validate Before Handoff"]
        CHK{Context Complete?}
        HC{Human Checkpoint\nReview Context Pack}
        READY[Handoff to\nRequirements Agent]
        CHK -->|incomplete| RANK
        CTX --> CHK
        CHK -->|complete| HC
        HC -->|approved| READY
        HC -->|revise| CHUNK
    end

    classDef ticketStyle fill:#1a1a2e,stroke:#4a9eff,color:#e0e0ff
    classDef indexStyle fill:#16213e,stroke:#4a9eff,color:#e0e0ff
    classDef assembleStyle fill:#0f3460,stroke:#7ec8e3,color:#e0e0ff
    classDef gateStyle fill:#0d2137,stroke:#7ec8e3,color:#e0e0ff
    classDef flagStyle fill:#2e0a0a,stroke:#e74c3c,color:#ffcccc
    classDef humanStyle fill:#2e1f00,stroke:#f5a623,color:#ffe0a0,stroke-width:2px
    classDef readyStyle fill:#0d2e1a,stroke:#2ecc71,color:#e0e0ff

    class TK,TM,AC,LK ticketStyle
    class QRY,HIT,MATCH indexStyle
    class RANK,CHUNK,CTX assembleStyle
    class CHK gateStyle
    class FLAG flagStyle
    class HC humanStyle
    class READY readyStyle
```

</details>

<details>
<summary><b>7. Agent Handoff Flow</b><br><i>The internal execution loop outlining how a single agent reasons, packages, and routes tasks.</i></summary>
<br>

```mermaid
flowchart TD
    subgraph LOAD["① LOAD — Agent Initialization"]
        CTX[Incoming Context Pack\nRequirements · Constraints · Schema]
        SF[Skill File\nRole Playbook · Workflow · Output Criteria]
        INIT[Agent Initialized\nRole · Tools · Memory · Goals]
        CTX --> INIT
        SF -->|loaded as system context| INIT
    end

    subgraph REASON["② REASON — LLM Layer"]
        PLAN[Planner\nDecompose Task · Sequence Steps]
        TOOLS[Tool Calls\nVector Index · APIs · Rule Engine]
        OUT[Raw Output\nDraft · Code · Decision · Diagram]
        INIT --> PLAN --> TOOLS --> OUT
        TOOLS -->|iterate| PLAN
    end

    subgraph PACKAGE["③ PACKAGE — Output Envelope"]
        RES[Result\nPrimary Deliverable]
        META[Metadata\nAgent Role · Skill File Version · Timestamp]
        CONF{Confidence\nThreshold Met?}
        ENV[Output Envelope\nResult · Metadata · Confidence · Handoff Notes]
        OUT --> RES
        OUT --> META
        OUT --> CONF
        CONF -->|no| PLAN
        RES --> ENV
        META --> ENV
        CONF -->|yes| ENV
    end

    subgraph ROUTE["④ ROUTE — Handoff Decision"]
        RTR{Next Step?}
        HC{Human Checkpoint\nReview Required?}
        NEXT[Next Agent\nLoads Envelope as Context]
        HUMAN[Human Review\nApprove · Revise · Escalate]
        DONE[Task Complete\nDeliver to Pipeline]
        ENV --> RTR
        RTR -->|agent handoff| HC
        HC -->|no| NEXT
        HC -->|yes| HUMAN
        HUMAN -->|approved| NEXT
        HUMAN -->|revise| PLAN
        RTR -->|terminal| DONE
        NEXT -->|next agent cycle| LOAD
    end

    classDef loadStyle fill:#1a1a2e,stroke:#4a9eff,color:#e0e0ff
    classDef reasonStyle fill:#0f3460,stroke:#7ec8e3,color:#e0e0ff
    classDef packageStyle fill:#1a0a2e,stroke:#9b59b6,color:#e0e0ff
    classDef routeStyle fill:#0d2137,stroke:#7ec8e3,color:#e0e0ff
    classDef humanStyle fill:#2e1f00,stroke:#f5a623,color:#ffe0a0,stroke-width:2px
    classDef doneStyle fill:#0d2e1a,stroke:#2ecc71,color:#e0e0ff
    classDef flagStyle fill:#2e0a0a,stroke:#e74c3c,color:#ffcccc

    class CTX,SF,INIT loadStyle
    class PLAN,TOOLS,OUT reasonStyle
    class RES,META,ENV packageStyle
    class CONF,RTR,HC routeStyle
    class HUMAN humanStyle
    class DONE doneStyle
    class NEXT loadStyle
```

</details>

<details>
<summary><b>8. Retry and Error Routing</b><br><i>The evaluation routing and remediation process for correctly handling and recovering from agent failures.</i></summary>
<br>

```mermaid
flowchart TD
    subgraph ENTRY["① ENTRY — Agent Output Received"]
        OUT[Agent Output\nResult · Metadata · Confidence]
        EVAL{Automated Eval\nRule Checks · LLM-as-Judge}
        OUT --> EVAL
    end

    subgraph TRIAGE["② TRIAGE — Classify the Failure"]
        PASS[Output Accepted\nThreshold Met]
        FAIL{Failure Type?}
        EVAL -->|pass| PASS
        EVAL -->|fail| FAIL
    end

    subgraph ROUTES["③ ROUTE — Send to Correct Handler"]
        LOW[Low Confidence\nAmbiguous Output]
        LOGIC[Logic Error\nIncorrect Reasoning]
        TOOL[Tool Failure\nAPI · Retrieval · Timeout]
        SCOPE[Scope Gap\nMissing Context or Artifacts]
        FATAL[Fatal Error\nUnrecoverable · Escalate]
        FAIL -->|confidence below threshold| LOW
        FAIL -->|incorrect reasoning| LOGIC
        FAIL -->|external dependency| TOOL
        FAIL -->|insufficient context| SCOPE
        FAIL -->|unrecoverable| FATAL
    end

    subgraph RECOVER["④ RECOVER — Remediation Actions"]
        R1[Retry with Refined Prompt\nAdjust Instructions · Narrow Scope]
        R2[Retry with Chain of Thought\nForce Step-by-Step Reasoning]
        R3[Retry Tool Call\nBackoff · Fallback Source]
        R4[Re-run Context Assembly\nExpand Retrieval · Flag Scope Gap]
        HC{Human Checkpoint\nEscalate for Review}
        LOW --> R1
        LOGIC --> R2
        TOOL --> R3
        SCOPE --> R4
        FATAL --> HC
        R1 -->|max retries exceeded| HC
        R2 -->|max retries exceeded| HC
        R3 -->|max retries exceeded| HC
        R4 -->|max retries exceeded| HC
    end

    subgraph RESOLVE["⑤ RESOLVE — Close the Loop"]
        RESUME[Resume Pipeline\nPass Corrected Output Forward]
        LOG[Log Failure Pattern\nFeed to Improve Stage]
        BLOCK[Block Pipeline\nAwait Human Decision]
        HC -->|approved| RESUME
        HC -->|unresolvable| BLOCK
        R1 -->|pass| RESUME
        R2 -->|pass| RESUME
        R3 -->|pass| RESUME
        R4 -->|pass| RESUME
        RESUME --> LOG
        BLOCK --> LOG
    end

    classDef entryStyle fill:#1a1a2e,stroke:#4a9eff,color:#e0e0ff
    classDef triageStyle fill:#16213e,stroke:#4a9eff,color:#e0e0ff
    classDef routeStyle fill:#2e1a0a,stroke:#f39c12,color:#ffe0cc
    classDef recoverStyle fill:#0f3460,stroke:#7ec8e3,color:#e0e0ff
    classDef resolveStyle fill:#0d2e1a,stroke:#2ecc71,color:#e0e0ff
    classDef failStyle fill:#2e0a0a,stroke:#e74c3c,color:#ffcccc
    classDef humanStyle fill:#2e1f00,stroke:#f5a623,color:#ffe0a0,stroke-width:2px
    classDef passStyle fill:#0d2e1a,stroke:#2ecc71,color:#e0e0ff

    class OUT,EVAL entryStyle
    class PASS,FAIL triageStyle
    class LOW,LOGIC,TOOL,SCOPE routeStyle
    class FATAL failStyle
    class R1,R2,R3,R4 recoverStyle
    class HC humanStyle
    class RESUME,LOG,BLOCK resolveStyle
    class PASS passStyle
```

</details>

<details>
<summary><b>9. Feedback Loop</b><br><i>The continuous improvement loop for feeding failure patterns and evaluations back into system components.</i></summary>
<br>

```mermaid
flowchart TD
    subgraph SIGNAL["① SIGNAL — Capture Feedback"]
        MON[Pipeline Monitor\nOutputs · Decisions · Failures]
        HF[Human Corrections\nApprovals · Revisions · Rejections]
        EVAL[Eval Scores\nLLM-as-Judge · Rule Check Results]
        DRIFT[Drift Detection\nDegraded Output Quality Over Time]
        MON --> HF
        MON --> EVAL
        MON --> DRIFT
    end

    subgraph CLASSIFY["② CLASSIFY — What Kind of Signal?"]
        RTR{Signal Type?}
        HF --> RTR
        EVAL --> RTR
        DRIFT --> RTR
        PROMPT[Prompt Issue\nInstructions Unclear · Incomplete]
        RETRIEVE[Retrieval Issue\nWrong Docs · Missing Context]
        SKILL[Skill File Issue\nRole Poorly Defined · Outdated]
        EVALDS[Eval Dataset Issue\nScoring Criteria Stale · Gaps]
        RTR -->|bad output pattern| PROMPT
        RTR -->|wrong context retrieved| RETRIEVE
        RTR -->|agent behavior drift| SKILL
        RTR -->|evals missing edge cases| EVALDS
    end

    subgraph IMPROVE["③ IMPROVE — Apply the Correction"]
        UP1[Refine Prompt\nUpdate Instructions · Add Examples]
        UP2[Update Retrieval Index\nRe-tag · Expand · Rerank]
        UP3[Update Skill File\nRevise Playbook · Add Constraints]
        UP4[Expand Eval Dataset\nAdd Failure Cases · Adjust Scoring]
        PROMPT --> UP1
        RETRIEVE --> UP2
        SKILL --> UP3
        EVALDS --> UP4
    end

    subgraph VALIDATE["④ VALIDATE — Confirm the Fix"]
        TEST[Regression Test\nRun Against Known Cases]
        HC{Human Checkpoint\nApprove Update?}
        SHIP[Ship Improvement\nDeploy to Live Pipeline]
        UP1 --> TEST
        UP2 --> TEST
        UP3 --> TEST
        UP4 --> TEST
        TEST --> HC
        HC -->|revise| IMPROVE
        HC -->|approved| SHIP
    end

    subgraph CLOSE["⑤ CLOSE — Feed Back Into the System"]
        IDX[Retrieval Index\nSharpened Embeddings]
        PROMPTS[Agent Prompts\nRefined Instructions]
        SKILLS[Skill Files\nUpdated Playbooks]
        EVALS[Eval Datasets\nExpanded Coverage]
        SHIP --> IDX
        SHIP --> PROMPTS
        SHIP --> SKILLS
        SHIP --> EVALS
        IDX -->|better retrieval| MON
        PROMPTS -->|better outputs| MON
        SKILLS -->|better behavior| MON
        EVALS -->|better scoring| MON
    end

    classDef signalStyle fill:#1a1a2e,stroke:#4a9eff,color:#e0e0ff
    classDef classifyStyle fill:#2e1a0a,stroke:#f39c12,color:#ffe0cc
    classDef improveStyle fill:#0f3460,stroke:#7ec8e3,color:#e0e0ff
    classDef validateStyle fill:#1a0a2e,stroke:#9b59b6,color:#e0e0ff
    classDef closeStyle fill:#0d2e1a,stroke:#2ecc71,color:#e0e0ff
    classDef humanStyle fill:#2e1f00,stroke:#f5a623,color:#ffe0a0,stroke-width:2px

    class MON,HF,EVAL,DRIFT signalStyle
    class RTR,PROMPT,RETRIEVE,SKILL,EVALDS classifyStyle
    class UP1,UP2,UP3,UP4 improveStyle
    class TEST,SHIP validateStyle
    class IDX,PROMPTS,SKILLS,EVALS closeStyle
    class HC humanStyle
```

</details>
