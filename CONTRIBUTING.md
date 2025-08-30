# Contributing to Shared Volume

Thank you for your interest in contributing to the Shared Volume project! This document provides guidelines and information for contributors to the entire Shared Volume ecosystem.

## 📚 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Project Overview](#project-overview)
- [How to Contribute](#how-to-contribute)
- [Development Setup](#development-setup)
- [Submitting Changes](#submitting-changes)
- [Style Guidelines](#style-guidelines)
- [Testing](#testing)
- [Documentation](#documentation)
- [Release Process](#release-process)
- [Getting Help](#getting-help)

## 📜 Code of Conduct

This project adheres to a code of conduct that we expect all contributors to follow. Please be respectful and constructive in all interactions.

### Our Standards

- Use welcoming and inclusive language
- Be respectful of differing viewpoints and experiences
- Gracefully accept constructive criticism
- Focus on what is best for the community
- Show empathy towards other community members

## 🏗️ Project Overview

The Shared Volume project consists of several components:

| Component | Repository | Description |
|-----------|------------|-------------|
| **shared-volume-controller** | [sharedvolume/shared-volume-controller](https://github.com/sharedvolume/shared-volume-controller) | Main controller managing SharedVolume and ClusterSharedVolume resources |
| **nfs-server-controller** | [sharedvolume/nfs-server-controller](https://github.com/sharedvolume/nfs-server-controller) | Optional NFS server controller |
| **volume-syncer** | [sharedvolume/volume-syncer](https://github.com/sharedvolume/volume-syncer) | Service for synchronizing data from external sources |
| **nfs-server-image** | [sharedvolume/nfs-server-image](https://github.com/sharedvolume/nfs-server-image) | Container image for NFS server deployment |
| **shared-volume-helm** | [sharedvolume/shared-volume-helm](https://github.com/sharedvolume/shared-volume-helm) | Helm charts for installation |

## 🤝 How to Contribute

### 🐛 Reporting Bugs

1. **Check existing issues** across all component repositories
2. **Identify the correct repository** for the bug
3. **Create a detailed issue** including:
   - Clear description of the bug
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment details (Kubernetes version, component versions, etc.)
   - Relevant logs or error messages
   - Component versions from `kubectl get pods -A`

### 💡 Suggesting Features

1. **Search existing issues** to see if the feature has been requested
2. **Consider which component** the feature belongs to
3. **Create a feature request** with:
   - Clear description of the feature
   - Use case and motivation
   - Possible implementation approach
   - Any alternatives considered
   - Impact on other components

### 🔧 Contributing Code

1. **Fork the appropriate repository**
2. **Create a feature branch** from `main`
3. **Make your changes**
4. **Add tests** for new functionality
5. **Ensure all tests pass**
6. **Update documentation**
7. **Submit a pull request**

### 📖 Contributing Documentation

- **User Documentation**: Update at [sharedvolume.github.io](https://sharedvolume.github.io)
- **API Documentation**: Update in component repositories
- **Examples**: Add to component `examples/` directories
- **README Updates**: Keep component READMEs synchronized

## 🛠️ Development Setup

### Prerequisites

- **Go 1.24+** (for controllers and syncer)
- **Node.js 16+** (for documentation site)
- **Docker**
- **kubectl**
- **kind** or access to a Kubernetes cluster
- **Helm 3.x**
- **make**

### Setting Up Your Development Environment

1. **Clone the main repository:**
   ```bash
   git clone https://github.com/sharedvolume/shared-volume.git
   cd shared-volume
   ```

2. **Initialize submodules:**
   ```bash
   git submodule update --init --recursive
   ```

3. **Set up a test cluster:**
   ```bash
   # Using kind
   kind create cluster --name shared-volume-dev
   
   # Install prerequisites
   kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.1/cert-manager.yaml
   
   # Install CSI NFS driver
   helm repo add csi-driver-nfs https://raw.githubusercontent.com/kubernetes-csi/csi-driver-nfs/master/charts
   helm install csi-driver-nfs csi-driver-nfs/csi-driver-nfs --namespace kube-system
   ```

4. **Choose your development focus:**

   **For shared-volume-controller:**
   ```bash
   cd shared-volume-controller
   make install  # Install CRDs
   make run      # Run controller locally
   ```

   **For nfs-server-controller:**
   ```bash
   cd nfs-server-controller
   make install
   make run
   ```

   **For volume-syncer:**
   ```bash
   cd volume-syncer
   go run cmd/server/main.go
   ```

   **For helm charts:**
   ```bash
   cd shared-volume-helm
   helm install shared-volume ./shared-volume --dry-run
   ```

### Development Workflow

1. **Work on individual components** in their respective directories
2. **Test integration** using the helm chart
3. **Update the main repository** documentation and examples
4. **Test the complete workflow** end-to-end

## 📝 Submitting Changes

### Pull Request Process

1. **Ensure your branch is up to date:**
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **Run component-specific tests:**
   ```bash
   # For Go components
   make test
   make fmt
   make vet
   make lint
   
   # For Helm charts
   helm lint ./shared-volume
   ```

3. **Test integration:**
   ```bash
   # Deploy your changes
   helm upgrade shared-volume ./shared-volume-helm/shared-volume
   
   # Test basic functionality
   kubectl apply -f examples/
   ```

4. **Commit your changes:**
   - Use clear, descriptive commit messages
   - Follow conventional commit format: `type(scope): description`
   - Examples:
     - `feat(controller): add support for SSH sync`
     - `fix(syncer): handle connection timeouts`
     - `docs(helm): update installation guide`

### Multi-Component Changes

For changes spanning multiple components:

1. **Create PRs in dependency order** (e.g., syncer → controller → helm)
2. **Reference related PRs** in descriptions
3. **Test integration** before merging
4. **Update main repository** documentation

## 🎨 Style Guidelines

### Go Code Style

- Follow standard Go formatting (`gofmt`)
- Use `golangci-lint` for linting
- Follow [Effective Go](https://golang.org/doc/effective_go.html) guidelines
- Use meaningful variable and function names
- Add comments for exported functions

### Kubernetes Resource Guidelines

- Follow [Kubernetes API conventions](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md)
- Use proper resource naming and labeling
- Include appropriate RBAC permissions
- Add proper validation and defaulting

### Documentation Style

- Use clear, concise language
- Include code examples
- Follow Markdown best practices
- Keep examples up to date

### Commit Message Format

Follow [Conventional Commits](https://conventionalcommits.org/):

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Formatting changes
- `refactor`: Code restructuring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Scopes:**
- `controller`: shared-volume-controller
- `nfs`: nfs-server-controller
- `syncer`: volume-syncer
- `helm`: helm charts
- `docs`: documentation
- `ci`: CI/CD changes

## 🧪 Testing

### Component Testing

Each component has its own testing requirements:

**Controllers (Go):**
```bash
make test                    # Unit tests
make test-integration        # Integration tests
make test-e2e               # End-to-end tests
```

**Volume Syncer (Go):**
```bash
go test ./...
go test -race ./...
```

**Helm Charts:**
```bash
helm lint ./shared-volume
helm template shared-volume ./shared-volume | kubeval
```

### Integration Testing

Test the complete workflow:

1. **Deploy all components:**
   ```bash
   helm install shared-volume ./shared-volume-helm/shared-volume
   ```

2. **Test basic functionality:**
   ```bash
   kubectl apply -f examples/basic-sharedvolume.yaml
   kubectl apply -f examples/test-pod.yaml
   ```

3. **Test external sync:**
   ```bash
   kubectl apply -f examples/git-sync-example.yaml
   ```

4. **Test NFS functionality:**
   ```bash
   kubectl apply -f examples/nfs-server.yaml
   ```

### Test Guidelines

- **Write tests for all new functionality**
- **Ensure tests are deterministic**
- **Use table-driven tests where appropriate**
- **Mock external dependencies**
- **Test both success and failure scenarios**

## 📚 Documentation

### Types of Documentation

1. **User Documentation** (sharedvolume.github.io)
   - Installation guides
   - User tutorials
   - API reference
   - Troubleshooting

2. **Developer Documentation**
   - Architecture decisions
   - Development setup
   - Component interactions
   - Testing strategies

3. **Code Documentation**
   - Godoc comments
   - Inline code comments
   - README files

### Documentation Guidelines

- **Keep documentation up to date** with code changes
- **Include examples** for all features
- **Explain the "why"** not just the "how"
- **Use diagrams** for complex concepts
- **Test documentation** by following your own guides

## 🚀 Release Process

### Component Releases

1. **Individual components** are released independently
2. **Follow semantic versioning** (vX.Y.Z)
3. **Create release notes** describing changes
4. **Tag releases** in git
5. **Update helm chart** dependencies

### Coordinated Releases

For major releases involving multiple components:

1. **Plan the release** across all components
2. **Update version compatibility** matrices
3. **Test integration thoroughly**
4. **Release in dependency order**
5. **Update main repository** documentation

### Release Checklist

- [ ] All tests passing
- [ ] Documentation updated
- [ ] CHANGELOG.md updated
- [ ] Version bumped appropriately
- [ ] Helm chart dependencies updated
- [ ] Integration tested
- [ ] Release notes prepared

## 💬 Getting Help

### Communication Channels

- **🐛 GitHub Issues** - Bug reports and feature requests
- **💬 GitHub Discussions** - General questions and community discussion
- **📧 Email** - bilgehan.nal@gmail.com for private inquiries

### Getting Support

1. **Search existing issues** across all repositories
2. **Check the documentation** at [sharedvolume.github.io](https://sharedvolume.github.io)
3. **Ask in GitHub Discussions** for general questions
4. **Create an issue** for bugs or specific problems

### Asking Good Questions

- **Include context** about what you're trying to achieve
- **Provide examples** of your configuration
- **Include error messages** and logs
- **Specify versions** of all components
- **Describe your environment** (Kubernetes version, platform, etc.)

## 🏆 Recognition

Contributors are recognized through:

- **GitHub contributor graphs**
- **Release notes** for significant contributions
- **Project README** acknowledgments
- **Community highlights** in discussions

### Types of Contributions

We value all types of contributions:

- 🐛 **Bug reports and fixes**
- ✨ **New features and enhancements**
- 📖 **Documentation improvements**
- 🧪 **Tests and quality improvements**
- 🎨 **User experience enhancements**
- 🤝 **Community support and engagement**

## 📋 Component-Specific Guidelines

For detailed component-specific contribution guidelines, see:

- [shared-volume-controller/CONTRIBUTING.md](shared-volume-controller/CONTRIBUTING.md)
- [nfs-server-controller/CONTRIBUTING.md](nfs-server-controller/CONTRIBUTING.md)
- [volume-syncer/CONTRIBUTING.md](volume-syncer/CONTRIBUTING.md)
- [nfs-server-image/CONTRIBUTING.md](nfs-server-image/CONTRIBUTING.md)

---

Thank you for contributing to Shared Volume! Your contributions help make Kubernetes volume management easier for everyone. 🎉

For questions about contributing, please don't hesitate to reach out through our communication channels.
