# Session 13 Assignment: Kubernetes Storage, HPA & Probes

| | |
|---|---|
| **Name** | Vanditabyaa Dwivedi |
| **Roll No.** | 24bcs10505 |
| **Session** | 13: Kubernetes Storage, HPA & Probes |
| **Environment** | macOS · Docker Desktop · Minikube (Kubernetes v1.37.0, 8 CPUs) · metrics-server addon |
| **Date** | 7 October 2026 |

All tasks were run on my local Minikube cluster. Every output and screenshot in this folder was captured from those runs.

## Contents

| Task | Folder | Summary |
|---|---|---|
| **1. Kubernetes Volumes** | [`01-kubernetes-volumes/`](01-kubernetes-volumes/) | emptyDir, hostPath, PV, PVC, StorageClass and dynamic provisioning: explanations plus a working demo of each |
| **2. HPA Hands-on** | [`02-hpa-hands-on/`](02-hpa-hands-on/) | Deployed the app and HPA, generated load, and watched CPU and pods scale **1 → 2 → 3 → 1** |
| **3. Mini Project** | [`03-mini-project/`](03-mini-project/) | Web app with a PVC, HPA and all 3 probes. Verified persistence, the Service, autoscaling (**2 → 3 → 2**), plus the bonus probe-failure challenges |

## Deliverables checklist

| Deliverable | Where |
|---|---|
| Volume documentation | [`01-kubernetes-volumes/README.md`](01-kubernetes-volumes/README.md) + [`manifests/`](01-kubernetes-volumes/manifests/) |
| HPA YAML | [`02-hpa-hands-on/hpa.yml`](02-hpa-hands-on/hpa.yml) (and [`03-mini-project/hpa.yaml`](03-mini-project/hpa.yaml)) |
| Load generator | [`02-hpa-hands-on/load-generator.yaml`](02-hpa-hands-on/load-generator.yaml) |
| HPA output | [`02-hpa-hands-on/outputs/`](02-hpa-hands-on/outputs/) and in the README |
| Screenshots | `screenshots/` folder inside each task (17 in total) |
| Mini-project implementation | [`03-mini-project/`](03-mini-project/) (namespace, pvc, deployment, service, hpa) |
| README documentation | One README per task, plus this index |

## Key results

- **emptyDir** data disappeared when the Pod was deleted. **hostPath** and **PVC** data survived Pod deletion.
- **Dynamic provisioning:** creating only a PVC made the `standard` StorageClass create a PV automatically within 4 seconds.
- **HPA:** 1 load generator gave 43% CPU, which is below the 50% target, so there was no scaling. 3 load generators gave 89% CPU, and the HPA scaled to **2 and then 3 replicas**. After the load was removed it scaled back to **1**.
- **Mini project:** a file containing my name and roll number survived Pod deletion and 3 rollouts. The HPA scaled 2 → 3 → 2.
- **Probes:** a broken readiness probe gave `0/1 Running` with no endpoints and no restarts. A broken liveness probe caused restarts every ~15s and then `CrashLoopBackOff`.

## How to reproduce

```bash
minikube start
minikube addons enable metrics-server
minikube addons enable default-storageclass
# then follow the README in each task folder
```
