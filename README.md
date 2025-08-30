<div align="center">
  <h1>🚀 Shared Volume</h1>
  <p><i>Effortless Kubernetes Volume Sharing & Synchronization</i></p>
</div>

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.24+-blue.svg)](https://kubernetes.io/)
[![Stars](https://img.shields.io/github/stars/sharedvolume/shared-volume?style=social)](https://github.com/sharedvolume/shared-volume/stargazers)

Shared Volume is a Kubernetes operator that enables easy sharing of volumes across pods in namespace or cluster scope, with optional data synchronization from external sources.

## 📖 Overview

Shared Volume provides Custom Resource Definitions (CRDs) that allow users to create shared storage solutions in Kubernetes clusters. The operator manages the lifecycle of shared volumes and automatically mounts them to pods through simple annotations.

### ✨ Key Features

- 📦 **Shared Storage**: Create volumes that can be shared across multiple pods
- 🌐 **Namespace & Cluster Scoped**: Support for both `SharedVolume` (sv, namespace-scoped) and `ClusterSharedVolume` (csv, cluster-scoped) resources
- 🔄 **External Data Sync**: Synchronize data from external sources (Git, S3, HTTP, SSH)
- 🎯 **Simple Integration**: Mount shared volumes to pods using simple annotations
- 🛡️ **NFS Support**: Optional NFS server management for enhanced storage capabilities
- ⚡ **Zero Configuration**: Works out of the box with minimal setup

### 💡 What is it good for

- 💾 **Efficient Storage Usage**: Uses the same storage at the background, keeping only one copy through shared storage approach - reducing storage costs and improving efficiency
- 🔌 **Simplified External Sync**: No need to implement custom solutions for syncing external data - just provide the parameters and use it for seamless integration with Git, S3, HTTP, or SSH sources
- 🔗 **Cross-Namespace Volume Sharing**: Enables volume sharing across namespace pods using ClusterSharedVolume, breaking traditional namespace boundaries for storage access
- 🎨 **Easy to Use**: No need to implement complex details - just add an annotation to your pod and the shared volume is automatically mounted

## 🧩 Components

The project consists of several components:

| Component | Version | Repository | Description |
|-----------|---------|------------|-------------|
| **shared-volume-controller** | [v0.1.0](https://github.com/sharedvolume/shared-volume-controller/releases/tag/v0.1.0) | [sharedvolume/shared-volume-controller](https://github.com/sharedvolume/shared-volume-controller) | Main controller managing SharedVolume and ClusterSharedVolume resources |
| **nfs-server-controller** | [v0.1.0](https://github.com/sharedvolume/nfs-server-controller/releases/tag/v0.1.0) | [sharedvolume/nfs-server-controller](https://github.com/sharedvolume/nfs-server-controller) | Optional NFS server controller for enhanced storage capabilities |
| **volume-syncer** | [v0.1.0](https://github.com/sharedvolume/volume-syncer/releases/tag/v0.1.0) | [sharedvolume/volume-syncer](https://github.com/sharedvolume/volume-syncer) | Service for synchronizing data from external sources |
| **nfs-server-image** | [v0.1.0](https://github.com/sharedvolume/nfs-server-image/releases/tag/v0.1.0) | [sharedvolume/nfs-server-image](https://github.com/sharedvolume/nfs-server-image) | Container image for NFS server deployment |
| **shared-volume-helm** | [v0.1.0](https://github.com/sharedvolume/shared-volume-helm/releases/tag/v0.1.0) | [sharedvolume/shared-volume-helm](https://github.com/sharedvolume/shared-volume-helm) | Helm charts for easy installation |

##  Documentation

For detailed documentation, installation guides, and examples, visit:

📖 **[sharedvolume.github.io](https://sharedvolume.github.io)**

## 🚀 Quick Start

```mermaid
flowchart LR
    A[📦 Install] --> B[📝 Create SV] --> C[🏷️ Add Annotation] --> D[🎉 Ready!]
```

**3 Simple Steps:**

### 📦 Installation

For detailed installation instructions, please visit our [Installation Guide](https://sharedvolume.github.io/installation).

### 💻 Basic Usage

1. **Create a SharedVolume (sv)**:
```yaml
apiVersion: storage.sharedvolume.io/v1alpha1
kind: SharedVolume
metadata:
  name: my-shared-volume
  namespace: default
spec:
  storage: 1Gi
  mountPath: "/opt/mnt/git-basic"
  syncInterval: "1m"
  syncTimeout: "30s"
  storageClassName: "standard"
  # Optional: sync from external source
  sync:
    source: git
    url: https://github.com/example/repo.git
```

2. **Use in Pod** (supports multiple volumes):
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  annotations:
    # Mount single SharedVolume
    sharedvolume.sv: "my-shared-volume"
spec:
  containers:
  - name: app
    image: nginx
    # Volumes will be automatically mounted at specified paths
```

## 🎯 Use Cases

- ⚙️ **Configuration Sharing**: Share configuration files and settings across multiple application pods
- 📦 **Big Size Common Used Data Sharing**: Distribute large datasets, models, or assets that need to be accessed by multiple pods
- 🔄 **Sync Data from External**: Automatically synchronize and share data from external sources (Git, S3, HTTP, SSH) across your workloads

## 📋 Requirements

- Kubernetes v1.24+
- Go 1.24+ (for development)

### 🔧 Prerequisites

- 🛡️ **cert-manager**: Required for TLS certificate management
- 💾 **csi-driver-nfs/csi-driver-nfs**: Required for NFS server functionality

## 🌐 Community & Support

- 🐛 **Issues**: [GitHub Issues](https://github.com/sharedvolume/shared-volume/issues)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/sharedvolume/shared-volume/discussions)
- 📧 **Email**: bilgehan.nal@gmail.com

## 📄 License

This project is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

Thanks to all [contributors](https://github.com/sharedvolume/shared-volume/graphs/contributors) who have helped make this project possible.

---

<div align="center">

**🌟 Star us on GitHub • 🐛 Report Issues • 💬 Join Discussions**

[![GitHub stars](https://img.shields.io/github/stars/sharedvolume/shared-volume?style=social)](https://github.com/sharedvolume/shared-volume/stargazers)
[![GitHub issues](https://img.shields.io/github/issues/sharedvolume/shared-volume)](https://github.com/sharedvolume/shared-volume/issues)
[![GitHub discussions](https://img.shields.io/github/discussions/sharedvolume/shared-volume)](https://github.com/sharedvolume/shared-volume/discussions)

[📖 Documentation](https://sharedvolume.github.io) • [⚡ Quick Start](#-quick-start) • [🤝 Contributing](CONTRIBUTING.md)

</div>