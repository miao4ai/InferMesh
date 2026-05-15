# InferMesh (v0.1-alpha)

> **"Everything infers locally. Infrastructure only orchestrates."**

InferMesh is a **local-first, distributed inference orchestration system**. It acts as the "Kubernetes for AI inference," abstracting heterogeneous edge hardware, local runtimes, and cloud fallbacks into a unified, goal-oriented inference mesh.

---

## 🚀 Core Philosophy

1. **Node-Centric, Not GPU-Centric:** Anything that can run an onion-skin model is an `Inference Node` (Jetson, Mac, Pi, Cloud GPU, Robot).
2. **Orchestration > Execution:** InferMesh does not build inference runtimes; it orchestrates existing ones (`llama.cpp`, `vLLM`, `ONNX Runtime`).
3. **Goal-Oriented API:** Developers don't call `run_model()`. They declare *Inference Goals* with strict constraints (latency, energy, cost).

---

## 🛠 System Architecture

```
┌────────────────────────────────────────────────────────┐
│                   Layer 4: App / SDK                   │
│          Goal-Oriented API: infer(goal, latency)       │
└───────────────────────────┬────────────────────────────┘
                            │
┌───────────────────────────▼────────────────────────────┐
│               Layer 3: Mesh Scheduler                  │
│       Topological Routing | Energy-Aware | Fallback    │
└───────────────────────────┬────────────────────────────┘
                            │
┌───────────────────────────▼────────────────────────────┐
│                Layer 2: Node Agent                     │
│    infermesh-agent | Capability Register | Health      │
└───────────────────────────┬────────────────────────────┘
                            │
┌───────────────────────────▼────────────────────────────┐
│               Layer 1: Local Runtime                   │
│        llama.cpp | ONNX Runtime | TensorRT             │
└────────────────────────────────────────────────────────┘

```

---

## 📂 Directory Structure

```text
infermesh/
├── control_plane/       # The Cluster Brain
│   ├── scheduler/       # Constraint-based node selector (latency, power)
│   ├── routing/         # Hierarchical routing (Local -> Edge -> Cloud)
│   └── topology/        # Mesh cluster node discovery & heartbeats
├── node_agent/          # Runs on EVERY device
│   ├── runtime/         # Drivers for llama.cpp, ONNX, vLLM
│   ├── health/          # Live hardware metric reporting (VRAM, Wattage)
│   └── cache/           # Local semantic cache & KV-cache slices
├── sdk/                 # Client API
│   └── python/          # Minimal goal-oriented inference SDK
└── examples/            # Heterogeneous Swarm Demos (Jetson + Mac + Pi)

```

---

## 🤖 API Abstraction (The Goal)

### 1. Basic Task Routing

```python
import infermesh as im

# Automatically routes to the optimal local node
result = im.infer(
    task="object_detection",
    input=image_bytes
)

```

### 2. Future-Proof Goal-Oriented Routing (The Core Vision)

```python
# Scheduler dynamically matches hardware capabilities & network latency
result = im.infer(
    goal="track_moving_object",
    context={"location": "camera_0"},
    constraints={
        "latency": "<50ms",
        "energy_mode": "low_power",
        "reliability": "critical"
    }
)

```

---

## 🎯 MVP v0.1 Scope

We are building the **minimal viable abstractions** first. Do not over-engineer.

* [ ] **Node Registry:** Simple agent that registers a node's static specs (Device Type, Available VRAM) and dynamic stats (Load).
* [ ] **Local-First with Cloud Fallback:** If a local node (e.g., Raspberry Pi) cannot satisfy the constraint or context length, the control plane intercepts and transparently proxies the request to a Cloud Node (e.g., OpenAI API / vLLM on Cloud).
* [ ] **Static Scheduler:** Simple cost/latency heuristic routing matrix.

---
