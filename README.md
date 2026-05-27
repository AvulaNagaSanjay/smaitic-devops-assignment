# Production-Ready Microservice Deployment

## Career Objective
SRE / DevOps professional with hands-on experience optimizing containerized workflows, building robust CI/CD pipelines, and managing distributed applications. Looking to leverage my infrastructure automation and reliability engineering expertise to build resilient, scalable, and secure cloud platforms that streamline delivery lifecycles.

---

## Architectural Decisions & Assumptions

* **Multi-stage Docker Build:** The original Dockerfile used a generic tag and ran everything in a single layer, which increases the image size and attack surface. I split this into a multi-stage build. The first stage handles the dependencies and compilation, while the final runner stage uses a minimal Alpine image (`node:20-alpine`) containing only production dependencies. 
* **Container Security:** To follow security best practices, the final image drops root privileges and switches to the built-in, unprivileged `node` user context. This prevents potential container breakout exploits in production.
* **Kubernetes Manifests:** I chose standard Kubernetes manifests over a Helm chart to keep things straightforward for a single, standalone microservice. This ensures clean separation and adheres strictly to the single-delivery constraint.
* **Custom Custom Port Name (`api-web`):** Per the infrastructure requirements, the container port inside the Deployment manifest is explicitly named `api-web` instead of standard names like `http`. The cluster IP service references this exact named target port to maintain seamless routing.
* **Observability Integration:** The application is assumed to output standard JSON logs to stdout/stderr, making it instantly compatible with the ELK stack (Logstash/Fluent-bit collection). The named port and deployment metadata are structured to allow a Prometheus agent to auto-discover and scrape application metrics cleanly.
