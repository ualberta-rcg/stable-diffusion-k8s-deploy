<img src="./assets/ua_logo_green_rgb.png" alt="University of Alberta Logo" width="50%" />

# Stable Diffusion Kubernetes Deployment

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
![Kubernetes](https://img.shields.io/badge/Kubernetes-manifest-326CE5?logo=kubernetes&logoColor=white)
![NVIDIA GPU](https://img.shields.io/badge/GPU-NVIDIA-76B900?logo=nvidia&logoColor=white)
![Repo Size](https://img.shields.io/github/repo-size/ualberta-rcg/stable-diffusion-k8s-deploy)
![Last Commit](https://img.shields.io/github/last-commit/ualberta-rcg/stable-diffusion-k8s-deploy)

**Maintained by:** Rahim Khoja ([khoja1@ualberta.ca](mailto:khoja1@ualberta.ca)) & Karim Ali ([kali2@ualberta.ca](mailto:kali2@ualberta.ca))

## 🧰 Description

A small Kubernetes manifest for running the AUTOMATIC1111 Stable Diffusion web UI on a GPU-enabled cluster.

This repository is intentionally lightweight. It captures the deployment that was used to get Stable Diffusion running quickly, including GPU scheduling, resource requests, model storage, and a LoadBalancer service.

## 🏗️ What It Deploys

`stable-diffusion-k8s.yaml` creates:

- A `stable-diffusion` namespace
- A single-replica `Deployment` using `goolashe/automatic1111-sd-webui:latest`
- NVIDIA GPU scheduling with `runtimeClassName: nvidia`
- A node selector for `NVIDIA-L40S` GPU nodes
- One requested GPU, with CPU and memory requests/limits
- An NFS-backed model mount at `/data/models/Stable-diffusion`
- A `LoadBalancer` service exposing the web UI on port `80`

The container runs with:

```bash
CLI_ARGS=--api
```

which enables the AUTOMATIC1111 API endpoint.

## ✅ Requirements

This manifest assumes the target Kubernetes cluster has:

- NVIDIA GPU nodes available
- The NVIDIA runtime class named `nvidia`
- GPU labels matching:

```yaml
nvidia.com/gpu.present: "true"
nvidia.com/gpu.product: NVIDIA-L40S
```

- An accessible NFS server at `global.storage.data.vulcan.local`
- Stable Diffusion models available under `/data/models/Stable-diffusion`
- LoadBalancer support, or a local equivalent such as MetalLB

Adjust the manifest before deploying if your cluster uses different GPU labels, runtime class names, NFS paths, or service exposure methods.

## 🚀 Deploy

Apply the manifest:

```bash
kubectl apply -f stable-diffusion-k8s.yaml
```

Check rollout status and service exposure:

```bash
kubectl -n stable-diffusion get pods
kubectl -n stable-diffusion get svc
```

Once the `LoadBalancer` gets an external address, open it in your browser to access the Stable Diffusion web UI.

## 📝 Notes

This is not a general-purpose Helm chart or production platform.

It is a simple, quick-start manifest for a known cluster layout. Review and tune the GPU selector, resource limits, NFS mount, and service type before using it in other environments.

## 🤝 Support

Many Bothans died to bring us this information. This project is provided as-is, but reasonable questions may be answered based on my coffee intake or mood. ;)

Feel free to open an issue or email **[khoja1@ualberta.ca](mailto:khoja1@ualberta.ca)** or **[kali2@ualberta.ca](mailto:kali2@ualberta.ca)** for U of A related deployments.

## 📜 License

This project is released under the **MIT License** - one of the most permissive open-source licenses available.

**What this means:**
- ✅ Use it for anything (personal, commercial, whatever)
- ✅ Modify it however you want
- ✅ Distribute it freely
- ✅ Include it in proprietary software

**The only requirement:** Keep the copyright notice somewhere in your project.

That's it! No other strings attached. The MIT License is trusted by major projects worldwide and removes virtually all legal barriers to using this code.

**Full license text:** [MIT License](./LICENSE)

## 🧠 About University of Alberta Research Computing

The [Research Computing Group](https://www.ualberta.ca/en/information-services-and-technology/research-computing/index.html) supports high-performance computing, data-intensive research, and advanced infrastructure for researchers at the University of Alberta and across Canada.

We help design and operate compute environments that power innovation — from AI training clusters to national research infrastructure.
