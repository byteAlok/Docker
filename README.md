# 🐳 Docker – Learning & Practical Series (Day 01)

A hands-on journey to learning Docker, containerization, and container-based application deployment through practical examples, commands, and real-world implementations.

## 📌 About This Repository

This repository documents my complete Docker learning journey, starting from the fundamentals of containerization to advanced Docker concepts and practical implementations.

Each topic is organized day-wise with detailed explanations, executable commands, examples, configurations, and hands-on exercises.

### 🎯 Learning Objectives

- 🐳 Understand Docker fundamentals and containerization.
- 📦 Work with Docker Images and Containers.
- ⚙️ Create and manage Dockerfiles.
- 🔗 Understand Docker Networking and Volumes.
- 🧩 Work with Docker Compose.
- 🔐 Learn Docker security and best practices.
- 🚀 Containerize and deploy real-world applications.
- ☁️ Integrate Docker with CI/CD and Cloud platforms.

---

## 📖 Introduction to Docker

**Docker** is an open-source platform that enables developers to build, package, distribute, and run applications inside isolated environments called **Containers**.

Docker solves a common development problem:

> "It works on my machine, but not on another machine."

Docker packages an application along with its required dependencies, libraries, and runtime environment into a Docker Image. This image can then be used to create and run containers consistently across different environments.

### 🖥️ Understanding Virtualization

Traditional virtualization uses a **Hypervisor** to create multiple Virtual Machines (VMs) on a physical machine.

Each Virtual Machine has its own Guest Operating System, kernel, libraries, and applications.

Docker uses a different approach called **OS-level Virtualization (Containerization)**.

Instead of creating a complete Virtual Machine for every application, Docker runs applications inside isolated containers that share the host operating system's kernel.

This makes containers lightweight, faster to start, and more resource-efficient than traditional Virtual Machines.

### ⚖️ Virtual Machines vs Docker Containers

| Virtual Machines | Docker Containers |
|---|---|
| Hardware-level virtualization | OS-level virtualization |
| Uses a Hypervisor | Uses a Container Engine |
| Each VM has its own Guest OS | Containers share the host kernel |
| Requires more resources | Generally lightweight |
| Slower startup | Faster startup |
| Hardware-level isolation | Process-level isolation |

**Note:** Docker containers are not completely independent operating systems. They share the host kernel. On Windows and macOS, Docker Desktop generally uses a lightweight Linux VM to run Linux containers.

### 🏗️ Basic Architecture

**Virtual Machine Architecture**

```text
Physical Infrastructure
        |
    Hypervisor
        |
  +-----+-----+
  |           |
 VM 1        VM 2
  |           |
Guest OS    Guest OS
  |           |
 App 1       App 2
```

**Docker Container Architecture**

```text
Physical Infrastructure
        |
     Host OS
        |
   Docker Engine
        |
  +-----+-----+
  |           |
Container 1  Container 2
  |           |
  App 1       App 2
  |           |
Dependencies Dependencies

   Shared Host Kernel
```

---

## 🛠️ Installation & Command References

Before starting the practical lessons, Docker must be installed and configured on your system.

### 1. Docker Installation Guide

📦 **Docker Installation – Step-by-Step Commands**

[DevOps Tools Installation – Docker.md](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/DevOps%20%26%20Infrastructure/Docker.md)

Contains Docker installation instructions, setup commands, and configuration steps.

### 2. Docker Command Handbook

⌨️ **Docker – Essential Command Reference**

[System Command Handbook – docker.md](https://github.com/byteAlok/System-Command-Handbook/blob/main/docker.md)

Contains approximately 25 commonly used Docker commands with their syntax and usage examples.

Useful for quick reference while performing Docker practicals and managing containers, images, networks, and other Docker resources.

---

## 📚 Learning Structure

All learning materials are organized day-wise.

Each day contains a Markdown file documenting the topics covered, commands executed, explanations, and practical results.

### 📂 Repository Structure

```text
Docker/
│
├── README.md              # Day 01 - Introduction to Docker
│
├── Day-02-Docker-Images-Containers-and-Terminal
|         |                           
|         └── README.md
│
├── Day-03-Dockerfile-Python-Deployment-and-Port-Mapping
|         |                           
|         └── README.md
│
├── Day-04-Dockerfile-Image-Build-and-Container-Execution
|         |                           
|         └── README.md
│
├── Day-05-Dockerfile-Port-ENTRYPOINT-AWS-ECR
|         |                           
|         └── README.md
│
├── Day-06-Multi-Stage-Docker-Build
|         |                           
|         └── README.md
│
├── Day-07-Docker-Networking-and-Docker-Volumes
|         |                           
|         └── README.md
│
├── Day-08-Docker-Compose-and-Docker-Swarm
|         |                           
|         └── README.md
│
└── ...
```

*Note: The above structure is an example. Actual files and folders will be added as new topics are covered.*

### 📝 Documentation Naming Convention

📚 All Docker learning topics, theoretical concepts, and practical implementations are documented in individual Markdown (`.md`) files.

📁 **No separate directories are created for individual days**, as the practical code and examples are limited. All documentation files are maintained directly inside the root `Docker/` directory.

#### 📄 Naming Format

```text
Day-<n>-<Topic-Name>/README.md
```

#### 💡 Examples

| Day | Documentation File |
|---|---|
| 📘 Day 01 | `README.md` |
| 🐳 Day 02 | `Day-02-Docker-Images-Containers-and-Terminal/README.md` |
| 🚀 Day 03 | `Day-03-Dockerfile-Python-Deployment-and-Port-Mapping/README.md` |
| 🛠️ Day 04 | `Day-04-Dockerfile-Image-Build-and-Container-Execution/README.md` |
| ☁️ Day 05 | `Day-05-Dockerfile-Port-ENTRYPOINT-AWS-ECR/README.md` |
| 📦 Day 06 | `Day-06-Multi-Stage-Docker-Build/README.md` |
| 🌐 Day 07 | `Day-07-Docker-Networking-and-Docker-Volumes/README.md` |
| ⚙️ Day 08 | `Day-08-Docker-Compose-and-Docker-Swarm/README.md` |

#### 📌 Documentation Guidelines

- 📖 **Day 01:** Root `README.md` represents **Introduction to Docker**.
- 📝 **Individual Files:** Every subsequent day has its own `.md` file.
- 🔧 **Theory + Practicals:** Both concepts and command-based practicals are documented in the same file.
- 📂 **Single Directory:** All Markdown files remain directly inside the root `Docker/` directory.
- ✨ **Consistent Naming:** Use descriptive, meaningful, and day-wise filenames for easy navigation.

**🎯 Goal:** Keep the repository simple, organized, and easy to navigate while maintaining all learning documentation in one place.

### 📄 Daily Documentation Includes

- 📖 Topic Introduction
- 🧠 Conceptual Explanation
- 💻 Commands and Syntax
- 🔍 Command Execution Results
- 🧪 Hands-on Practicals
- 📌 Important Notes and Learnings

---

## 🚀 Learning Approach

This repository follows a practical, step-by-step learning approach.

1. Understand the concept.
2. Learn the required commands and syntax.
3. Execute commands in a practical environment.
4. Observe and analyze the results.
5. Document the learning and implementation.

The goal is not just to understand Docker theoretically, but to gain practical experience with containerization and real-world DevOps workflows.

---

## 🔗 Useful Resources

- 📘 [Docker Official Documentation](https://docs.docker.com/)
- 🚀 [Docker Get Started](https://docs.docker.com/get-started/)
- 📦 [Docker Hub](https://hub.docker.com/)
- 🛠️ [Docker Installation Guide](https://github.com/byteAlok/DevOps-Tools-Installation-Commands/blob/main/DevOps%20%26%20Infrastructure/Docker.md)
- ⌨️ [Docker Command Handbook](https://github.com/byteAlok/System-Command-Handbook/blob/main/docker.md)

---

## 👨‍💻 Author

**Alok Maurya**

- 🐙 GitHub: [@byteAlok](https://github.com/byteAlok)
- 🌐 Portfolio: [aloksde.com](https://aloksde.com)

💡 Learning | Building | Experimenting | Growing

---

⭐ If you find this repository helpful, consider giving it a star!
