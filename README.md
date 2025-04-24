# ToolSelect TAO — Proposal
A specialised Bittensor agent that decides **which miner/API to call** for any task, in real time. It removes discovery code from every other agent and turns network complexity into an efficiency gain.

---
## 1 Agent Snapshot
| Field | Detail |
|-------|--------|
| **Name** | **ToolSelect TAO** |
| **Role** | Task‑to‑tool router and cost/latency forecaster |
| **Interface** | JSON‑RPC (gRPC optional) |
| **Average response** | ≤150 ms including network hop |

---
## 2 Subnets Leveraged
| Subnet (example ID) | Purpose |
|--------------------|---------|
| **Language (1, 3)** | Parse task text & miner metadata |
| **Retrieval (4)** | Fetch fresh Web & on‑chain data |
| **Stats & Analytics (7)** | Stream live latency, cost, reliability |
| **Meta‑Governance (11)** | Publish signed outcome proofs for validators |
| **Future subnets** | Auto‑discovered if capability metadata is exposed |

---
## 3 Problem Recap
Special‑purpose miners are multiplying fast. Agents that search alone:
* Burn extra CPU/VRAM.
* Add 50–500 ms latency per call.
* Often pick cost‑inefficient or unstable miners.

---
## 4 Architecture – Core Parts
1. **Input Parser** – normalises `{task, context, priorities}`.
2. **Knowledge Collector** – scrapes Stats subnet & pings miners for live health.
3. **Ranking Engine** – v1 heuristics, v2 multi‑armed bandit; scores candidates.
4. **Response Builder** – returns `{tool_id, cost_tao, latency_ms, confidence}`.
5. **Proof Publisher** – posts anonymised success/fail + latency to Meta‑Governance for validator rewards.

---
## 5 Interaction Protocol
### Request
```json
{
  "task": "extract_entities",
  "context": { "text": "<article here>" },
  "candidates": ["miner_uid_1", "miner_uid_2"],
  "priority": ["accuracy", "cost"],
  "constraints": { "max_latency_ms": 400 }
}
```
### Response
```json
{
  "tool_id": "miner_uid_1",
  "cost_tao": 0.0001,
  "latency_ms": 280,
  "confidence": 0.92,
  "reason": "highest accuracy while under latency cap"
}
```

---
## 6 Deployment Models
| Model | Pros | Cons |
|-------|------|------|
| **Dedicated subnet** | Built‑in validator scoring; emission rewards; clear marketplace | Bootstrapping effort |
| **Cross‑subnet service** | Easy to launch; fee‑only | Discovery & quality scoring are external |

Either path keeps multiple miners competing on algorithm quality—no central choke‑point.

---
## 7 Algorithms & Knowledge‑Base
* **Heuristics**: cheap rules for cold‑start.
* **Contextual Bandits**: adapt choice based on task & feedback.
* **Embedding similarity**: task and miner capability vectors.
* **Reinforcement Learning**: long‑horizon reward – lower total TAO consumed.
* **KB refresh**: combine on‑chain miner metadata, ping results, and validator proofs; stale records downgraded automatically.
* **Anti‑gaming**: cross‑check miner self‑reports with observed latency & success; proofs signed and public.

---
## 8 Use Cases & Products
| Audience | Product | Impact |
|----------|---------|--------|
| **Agent devs** | `@toolselect/cli` + JS SDK | One‑liner smart routing; less code.
| **Chatbots** | Middleware | Live route news‑vs‑price queries to best miners.
| **Data pipelines** | Airflow / Prefect plugin | Dynamic DAG: clean → analyse → summarise with cheapest fast miners.
| **Enterprise** | On‑chain licence | Predictable spend & audit trail.
| **Miners** | Quality leaderboard | Higher utilisation for high‑score miners.

---
## 9 Economy
* **Fee‑for‑service**: 0.00005 TAO or 0.5 % of downstream fee, whichever higher.
* **Selector miners**: fee + optional emissions (dedicated subnet).
* **Pay‑for‑success**: clients refunded if selected miner fails (via proof).

TAO flow:
```
User → Client Agent → ToolSelect TAO (fee) → Chosen Miner (exec fee) → Validators (emission)
```

---
## 10 Illustrative Cost Impact
| Task type | Internal search compute | Internal latency | Delegated latency | Fee (TAO) | Net benefit |
|-----------|------------------------|------------------|-------------------|-----------|-------------|
| Simple (≤5 candidates) | 5 units | 50 ms | 75 ms | 0.00001 | Neutral |
| Moderate (≤50) | 50 units | 300 ms | 150 ms | 0.00005 | 40 % faster, big compute save |
| Complex (≥200) | 200 units | 1000 ms | 300 ms | 0.0001 | 70 % faster, major compute save |

---
## 11 Naming Rationale
| Candidate | Signal | Why rejected |
|-----------|--------|--------------|
| **ToolSelect TAO** | Function‑first, TAO‑native | **Chosen**. Clear & memorable. |
| Orchestrator Agent | Suggests large‑scope workflow | Too broad; could dilute focus. |
| FunctionRouter TAO | Accurate but less intuitive | Slightly abstract for newcomers. |

---
## 12 Validator Metrics
| Metric | Source | Weight |
|--------|--------|-------|
| Success rate | Proof Publisher logs | 40 % |
| Median latency | Proof logs | 30 % |
| Cost efficiency | TAO spent / success | 20 % |
| Client feedback | Optional rating | 10 % |

---
## 13 Value for Bittensor
* **Network efficiency:** 10‑30 % cheaper tasks in pilot sims.
* **Composability:** agents chain new subnets instantly.
* **Miner revenue:** quality surfaces automatically.
* **Token utility:** every selection is a micro‑transaction.
* **Governance data:** hard numbers to reward performance.

---
## 14 Bottom Line
ToolSelect TAO is the routing layer that lets every other agent run lean while the network keeps scaling.

