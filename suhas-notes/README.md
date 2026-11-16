# suhas notes — converted from OneNote print-outs

Every notebook in this folder was exported from OneNote as a vector PDF
(Chrome/Skia print, 841.92 × 594.96 pt landscape) and converted to a
**native, editable Xournal++ `.xopp`**. All ink is preserved as real
strokes (original colours, widths and positions), and the light-blue
graph-paper grid is rebuilt as a separate `grid` layer you can toggle off.

Blank OneNote pages left in the exports were dropped. Source PDFs are kept
alongside each `.xopp`.

Files are sorted and named after **what is written on the pages**, not
the OneNote titles, because several titles did not match the content.

## Index

### Security
- **Authentication - Password Hashing, Salts & JWT.xopp** (14 pages, was
  `suhas notes_auth.pdf`, notebook title "Auth", 2026-11-03 21:51)
  - Sign-up / login flows on paper: validate input → check user exists →
    generate salt → hash(pw+salt) → save; email / time / salt / password
    hash split from the stored record
  - Hashing vs encoding vs encryption (one-way vs two-way + key)
  - Why hash and not encrypt; bcrypt / Argon2, HTTPS, no lookalike DB
    or email-side hashing; server stores only salted hash
  - "Same Pw → same salt+hash" and how that prevents plain-password reuse
  - Auth flow + login flow; JWT as stateless transport

### AI and Agents
- **AI System Design Approach - QSN & RAG Reference Architecture.xopp**
  (3 pages, was `suhas notes_approachtosystemdesign.pdf`, notebook title
  "Approach to SD QSN", 2026-11-12 22:40)
  - "Things to ask" checklist for system-design interviews: scoping the
    proper requirement, numbers, tradeoffs, verification; follow the
    natural order: clarity → count → baseline → draw → dive → prove →
    de-risk
  - AI/RAG reference architecture: client → gateway (auth + limit) →
    router (query difficulty) → context / RAG tier → LLM (fast vs slow
    lane) guard → response; "Flows & Requirements" notes about traces
    (full logging), eval sets, quality/cost/latency/safety
  - Retrieval funnel (chunks → top-50 cross-encoder → top-6 → prompt),
    escalation ladder (prompt → RAG → tools/agent loop → LoRA fine-tune),
    agent-loop decay (budget/max-steps, checkpoints, capability caps),
    safety stack (jailbreak, prompt injection, SQL injection, critical
    info, sandbox, human-approval gate for irreversible actions)
  - "Question - Scenario": a bank AI support assistant (8M users,
    hundreds of thousands of logs, P95 latency, weekly deploys,
    compliance-related risks, ₹-cost/convo target)
- **SuperAgents - Persistent AI Agent Architecture.xopp**
  (6 pages, was `suhas notes_superagents.pdf`, notebook title
  "SuperAgents", 2026-11-03 18:35)
  - SuperAgents handle multiple sessions, are event-driven and take
    signals from a configured env to act accordingly; defined as a
    "continuous entity synced across all tasks"
  - Principles: continuous / persistent / reactive / productive;
    architecture: trigger → 2-stage filter → routing layer (global;
    internal layer receives only trigger from anywhere) → interim and
    worker agents; "if a subagent is already present, pass through"
  - Fast path (`@agent` deterministic routing) vs slow path (large
    semantic LLM); event routing direct-to-worker when the event already
    has one; claims thread ≈ worker agent
  - Context organizer (spill to vector store, cache, knowledge graph,
    "chain of recall"), speculative executions / background confirm,
    half-duplex vs full-duplex multi-channel (instant vs async channels),
    separate compute / state / env (stateless workers, agent state in DB),
    watchdog monitoring tasks and infra ("always woken")
- **Vector Databases - Embeddings & FAISS.xopp**
  (1 page, was `suhas notes_vectordbs.pdf`, notebook title
  "Vector databases", 2026-11-06 19:11)
  - Agentic search vs keyword search vs no-retrieval; embeddings →
    chunks → vector → query → similarity search; swapping the embedding
    model changes the maths ("changing emb model adds opacity")
  - Vector DB = complete software vs vector (index) algorithm/DS:<br>
    Pinecone-style full stack vs pgvector (vector search + SQL) vs
    Chroma-style points-vector/single instance → quick prototyping;
    relevant documents sent directly to the model (affects accuracy)
  - FAISS (Facebook AI Similarity Search): `IndexFlatL2` brute-force
    Euclidean search (exact, small DB); `IndexIVF` inverted file index
    (Flat/FPQ; only the matching cells are searched); `HNSW` graph-based
    "graph of vectors"; `PQ` product quantization; comparison table
    Flat / IVF / HNSW / PQ / IVF-PQ → brute force / flat+clustering /
    graph / compressed / cluster+compressed

### System Design
- **Network Latency Scenario - DNS, TLS, Edge & Caching.xopp**
  (2 pages, was `suhas notes_netowrk.pdf` (typo in OneNote), notebook
  title "Network and latency", 2026-11-12 22:23)
  - Scenario decomposition: decompose the request, check the user (bool,
    ≈0.4 ms); the "4-step method" for performance work (break into small
    phases, measure everything, find the dominant one, fix / remeasure /
    repeat)
  - DNS (no→add, 3-4 RTT, cache) → TCP handshake (1 RTT) → TLS 1.3
    (1-2 RTT) → server responds (off-origin 60 ms) → transfer with
    TCP slow start; warm up connections; edge (send-first / cache)
    across regions
  - Scenario DST tree: "slow be whom?" far vs near; first asset vs last
    (edge storage / edge) vs big vs small region (bandwidth, slow
    start, warm start, compressed, NMA corr?)

### Hardware
- **Semiconductors & Chip Hardware - Silicon, GaN & Diamond.xopp**
  (1 page, was `suhas notes_chips_hardware.pdf`, notebook title
  "Chips and hardware", 2026-11-03 17:18)
  - Voltage, silicon, gallium nitride; silicon vs atom-bandgap;
    conduction ⇌ valence; diamond (hard to work), silicon dioxide /
    germanium dioxide (melting material / droplets); copper → light is
    used for data movement

### Robotics and Machine Learning
- **Behavioral Cloning - Imitation Learning.xopp**
  (1 page, was `suhas notes_robotics.pdf`, notebook title
  "Behavioral cloning", 2026-11-06 21:59)
  - Why BC replaced RL: available robots, data-rich; RL is not enough
    (local / specific circumstances, missing end-to-end correctness,
    "Analytical" mismatch)
  - Difficulties with reward shaping (complex, error-prone, hard to
    debug) and sim ≠ real; survey refs "RL in robotics (survey paper)"
    and "DAL-for-robotics (survey of real world successes)";
    "Learning to recover from failure"
  - Off-policy; entropy-regularized extrapolation error; learned
    rewards; human-in-the-loop; HI-SERL; model coupling → reroute
    (plain/latent(?)); ACT (modern BC policies); diffusion policy;
    demos are not golden and are inherently multimodal; joint/task
    space notes (cartesian vs dense space reward)

## How these were made
OneNote print PDFs (Skia/Chrome, vectorized strokes — no rasterized
handwriting) were converted by a custom parser
(`/home/ssp/projects/suhas-notes-convert/pdf_to_xopp.py`): every stroked
PDF path → an Xournal++ pen stroke with the same colour, width and
points; the light-blue grid → a separate `grid` layer; the full-page
white fill → the background; the OneNote page-title/timestamp → a text
element. Files are written as gzipped XML exactly like Xournal++ writes
(the Android app refuses plain-XML `.xopp`). Bookkeeping:

| File | PDF pages | XOPP pages | ink strokes (approx.) | blank pages dropped |
|---|---|---|---|---|
| Authentication… | 15 | 14 | ~1500 | 1 |
| AI System Design Approach… | 3 | 3 | ~1040 | 0 |
| SuperAgents… | 6 | 6 | ~1940 | 0 |
| Vector Databases… | 2 | 1 | ~870 | 1 |
| Network Latency Scenario… | 2 | 2 | ~550 | 0 |
| Semiconductors… | 1 | 1 | ~200 | 0 |
| Behavioral Cloning… | 2 | 1 | ~590 | 1 |

Every `.xopp` was re-opened and exported by Xournal++ 1.3.8 (Fedora) with
no warnings; strokes sit at the original coordinates and colours, so
pages are byte-faithful to the PDF render.

Notes:
- The handwritten text is ink, not typeset text — fully editable /
  erasable, never converted to OCR text.
- Source PDFs are kept next to the `.xopp` files; delete them if unwanted.
- To add more notebooks later, export them from OneNote to PDF the same
  way and run:

  `python3 /home/ssp/projects/suhas-notes-convert/pdf_to_xopp.py "Notebook.pdf" "Notebook.xopp" "Title"`
