# Production-Ready Microservice Deployment

## Career Objective
To leverage my infrastructure automation and reliability engineering expertise to build resilient, scalable, and secure cloud platforms that streamline delivery lifecycles.

---

## Architectural Decisions & Assumptions

* **Multi-Stage Containerization**: Created a discrete builder step to compile assets, isolating development baggage from the ultimate production image[cite: 7, 8]. The run layer uses `node:20-alpine` to maintain a minimal vulnerability profile and fast image transfer speeds.
* **Security & Non-Root Execution**: Altered execution contexts from standard `root` to the integrated `node` security user context to prevent container breakout risks.
* **Declarative Manifest Delivery**: Selected vanilla Kubernetes specs over Helm to keep configuration structures clean for a single standalone microservice and strictly adhere to submission safety rules.
* **Named Ingress Ports**: The pod endpoint is explicitly labeled `api-web` across deployments and load-balancing abstractions to fulfill target routing mandates[cite: 28, 29].
* **Observability Readiness**: Pod configurations expose named ports mapping to targeted standard streams, facilitating collection by Prometheus metric endpoints and the ELK stack pipeline[cite: 20, 21].
