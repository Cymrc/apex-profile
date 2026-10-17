# apex-profile

Kernel-level eBPF tracing for database query stalls, lock contention, and unindexed joins with sub-1% CPU overhead.

> **⚠️ Notice:** *This platform and its repository files are strictly intended for a mini-project.*

---

## 🔗 Live Demo

- **Inspection Console Preview:** [ApexProfile Diagnostic Dashboard](https://cymrc.github.io/apex-profile/)

---

ApexProfile attaches to Linux socket syscalls (`sys_enter_recvfrom`, `sys_enter_writev`) via eBPF kprobes. By observing wire protocols inside kernel memory buffers, it inspects queries and records pipeline latency without application-level agent wrappers or code modifications.

---

## 🗺️ Capabilities

- **Socket-layer observation:** Parses wire-protocol frames for PostgreSQL, MySQL, and Redis before runtime serialization.
- **Lock contention tracing:** Correlates execution wall-clock time with thread lock states to isolate row-exclusive locks from heap scans.
- **Bounded overhead:** Uses non-blocking ringbuffer pushes with a CPU budget under 0.8% at 65,000 requests per second.
- **No runtime wrappers:** Runs entirely outside user-space runtimes to prevent garbage-collection pause spikes and thread disruption.

---

## 🛠️ Technical Constraints

| Subsystem | Supported target | Hook mode / driver |
|---|---|---|
| **Linux Kernel** | 5.8+ (`x86_64`, `aarch64`) | CO-RE enabled (`BTF` required) |
| **Container Runtimes** | containerd, CRI-O, Docker Engine | `CAP_SYS_ADMIN`, `CAP_BPF` |
| **Export Transport** | OTel gRPC, Prometheus scrape, JSONL | Non-blocking ringbuffer drain |

---

## 🚀 Quickstart

### 1. Install the collector daemon

Run the installation script on the target host:

```bash
curl -sSf [https://install.apexprofile.internal/collector.sh](https://install.apexprofile.internal/collector.sh) | sh
