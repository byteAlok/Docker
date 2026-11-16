# 🐳 Day 02 – Docker Images, Containers & Terminal

> 🚀 Learning Docker through hands-on practice and understanding Images, Containers, and Interactive Terminal operations.

---

## 📚 Topics Covered

- 🌐 Docker Hub & Local Images
- 📥 Pulling Docker Images
- 📋 Listing Docker Images
- 📂 Docker Storage Path
- 🖥️ Docker Version
- 📦 Docker Image vs Container
- 🚀 Creating Containers using `docker run`
- 🔀 Detached Mode (`-d`)
- 💻 Interactive Mode (`-it`)
- 🔑 Accessing Containers using `docker exec`
- ⚙️ Understanding Container Processes
- 🔍 Listing Running & Stopped Containers
- 🛠️ Inspecting Docker Images & Containers
- 📜 Viewing Container Logs

---

## 1. 🌐 Docker Hub & Local Images

### ☁️ Docker Hub

Docker Hub is a cloud-based container image registry where developers can store, share, and download Docker images.

It provides ready-to-use images for operating systems, databases, programming languages, and applications.

### 💻 Local Images

Local Images are Docker images already downloaded and available in the local Docker environment.

### 📥 Pull Ubuntu Image

```bash
docker pull ubuntu
```

**🔍 Why use it?**

Downloads the Ubuntu image from Docker Hub to the local Docker environment.

**⚙️ Command Breakdown:**

| Command | Description |
|---|---|
| `docker` | Docker CLI |
| `pull` | Downloads an image |
| `ubuntu` | Image name |

**📤 Example Output:**

```text
Using default tag: latest
latest: Pulling from library/ubuntu
...
Status: Downloaded newer image for ubuntu:latest
```

### 📋 List Docker Images

```bash
docker images
```

**🔍 Why use it?**

Displays all Docker images currently available locally.

**📤 Example Output:**

```text
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    abc123         ...           25.84MB
```

| Field | Description |
|---|---|
| 📦 REPOSITORY | Image name |
| 🏷️ TAG | Image version or variant |
| 🆔 IMAGE ID | Unique image identifier |
| 📅 CREATED | Image creation information |
| 💾 SIZE | Image size |

### 📥 Pull Amazon Linux Image

```bash
docker pull amazonlinux
```

Downloads the Amazon Linux image from Docker Hub.

### 🖥️ Check Docker Version

```bash
docker version
```

Displays Docker Client and Docker Server (Daemon) version information.

---

## 2. 📂 Docker Storage Path

Docker uses a storage directory to maintain its internal data, including downloaded images, containers, and other Docker-managed resources.

**📍 Default Linux Docker Storage Path:**

```text
/var/lib/docker
```

**💡 Why is it important?**

- 📦 Stores Docker-managed image data.
- 🗂️ Maintains container-related data.
- ⚙️ Stores other internal Docker information.

---

## 3. 📦 Docker Image vs Container

### 🖼️ Docker Image

A Docker Image is a read-only template containing the filesystem and configuration required to create a container.

It provides the application environment, libraries, dependencies, and default execution settings.

### 📦 Docker Container

A Docker Container is an isolated runtime instance created from a Docker Image.

It provides an environment in which the configured application or process can execute.

### 🔄 Image to Container

```text
       🖼️ Docker Image
           (Ubuntu)
              |
              |
        🚀 docker run
              |
              ▼
       📦 Docker Container
           (Ubuntu)
```

### ⚖️ Key Differences

| 🖼️ Docker Image | 📦 Docker Container |
|---|---|
| Read-only template | Runtime instance |
| Used to create containers | Created from an image |
| Contains application environment | Runs configured processes |
| Can create multiple containers | Has its own runtime state |

### 🚀 Create a Container

```bash
docker run ubuntu
```

**🔍 Why use it?**

Creates and starts a new container using the Ubuntu image.

**⚙️ How it works:**

1. 🔎 Docker checks whether the image exists locally.
2. 📥 If unavailable, Docker attempts to download it.
3. 📦 Docker creates a new container.
4. 🚀 Docker starts the configured main process.

**⚠️ Important:** When the container's main process exits, the container stops.

---

## 4. 🔀 Detached Mode (`-d`)

```bash
docker run -dt ubuntu
```

**📖 Definition:**

Detached mode allows a container to run in the background without attaching the current terminal to its standard input and output.

It is useful when we want to start a container and continue using the host terminal.

### ⚙️ Command Breakdown

| Part | Description |
|---|---|
| `docker` | Docker CLI |
| `run` | Creates and starts a container |
| `-d` | Runs in detached mode |
| `-t` | Allocates a pseudo-terminal |
| `ubuntu` | Docker image name |

**📤 Example Output:**

```text
a1b2c3d4e5f6...
```

Docker generally returns the container ID.

**💡 Remember:**
- 🔀 `-d` → Detached mode.
- 🖥️ `-t` → Terminal allocation.

**⚠️ Note:** Detached mode does not guarantee that a container will keep running. Its main process must remain active.

---

## 5. 💻 Interactive Mode (`-it`)

```bash
docker run -it ubuntu
```

**📖 Definition:**

Interactive mode allows users to interact directly with a container through a terminal.

The `-i` and `-t` options work together to provide an interactive terminal experience.

### ⚙️ Command Breakdown

| Part | Description |
|---|---|
| `docker` | Docker CLI |
| `run` | Creates and starts a container |
| `-i` | Keeps standard input open |
| `-t` | Allocates a pseudo-terminal |
| `ubuntu` | Docker image name |

**📤 Expected Terminal:**

```text
root@a1b2c3:/#
```

Now we are inside the Ubuntu container.

### 💻 Execute Linux Commands

```bash
pwd
whoami
ls
```

**💡 Remember:**

- ⌨️ `-i` → Interactive input.
- 🖥️ `-t` → Terminal allocation.
- 🔗 `-it` → Interactive terminal.

---

## 6. 🔑 Accessing an Existing Container – Docker Exec

Sometimes we start a container in detached mode and later need to access its terminal.

For this purpose, Docker provides the `exec` command.

### 📝 Command Syntax

```bash
docker exec -it <container_name_or_id> <command>
```

**📖 Definition:**

`docker exec` executes a new command inside an already running container without creating another container.

### ⚙️ Command Breakdown

| Part | Description |
|---|---|
| `docker` | Docker CLI |
| `exec` | Executes a command inside an existing container |
| `-i` | Keeps standard input open |
| `-t` | Allocates a pseudo-terminal |
| `container_name_or_id` | Container name or ID |
| `command` | Command to execute |

### 💡 Example

```bash
docker exec -it myubuntu /bin/bash
```

**🔍 Explanation:**

- 📦 `myubuntu` → Existing container name.
- 🐚 `/bin/bash` → Bash shell executable.
- 💻 `-it` → Interactive terminal access.

### ⚖️ Docker Run vs Docker Exec

```bash
docker run -it ubuntu
```

🆕 Creates a new container and opens its interactive terminal.

```bash
docker exec -it myubuntu /bin/bash
```

🔑 Opens a Bash shell inside an already running container.

---

## 7. ⚙️ Understanding Container Processes

A process is a running instance of a program or command.

Containers execute processes, and Linux provides commands to examine them.

### 📋 List Running Processes

```bash
ps aux
```

**📖 Definition:** Displays information about currently running processes in the current environment.

**⚙️ Command Breakdown:**

| Option | Description |
|---|---|
| `ps` | Displays process status |
| `a` | Includes processes associated with terminals |
| `u` | User-oriented format |
| `x` | Includes processes without controlling terminals |

### 🔢 Count Process Lines

```bash
ps aux | wc -l
```

**📖 Definition:** Passes the process listing to `wc -l` to count the number of lines.

**⚙️ Command Breakdown:**

| Part | Description |
|---|---|
| `ps aux` | Displays process information |
| `\|` | Pipe operator |
| `wc` | Word count utility |
| `-l` | Counts lines |

**📤 Example Output:**

```text
118
```

The actual count may vary depending on the environment and running processes.

---

## 8. 🔍 Docker Container Management

### 🟢 List Running Containers

```bash
docker ps
```

**📖 Definition:** Displays currently running Docker containers.

**🔍 Why use it?**

To check active containers and their IDs, image names, commands, and status.

**📤 Example Output:**

```text
CONTAINER ID   IMAGE    COMMAND          STATUS
abc123         ubuntu   "/bin/bash"      Up 2 minutes
```

### 📋 List All Containers

```bash
docker ps -a
```

**📖 Definition:** Displays all containers regardless of their current state.

Includes:

- 🟢 Running containers.
- 🔴 Exited containers.
- 🟡 Created containers.

### 🔴 Filter Exited Containers

```bash
docker ps -a | grep Exited
```

**📖 Definition:** Combines Docker container listing with Linux `grep` to filter entries containing the word `Exited`.

**⚙️ Command Breakdown:**

| Part | Description |
|---|---|
| `docker ps -a` | Lists all containers |
| `\|` | Passes output to the next command |
| `grep` | Searches matching text |
| `Exited` | Text to search for |

---

## 9. 🔎 Docker Inspect

```bash
docker inspect ubuntu
```

**📖 Definition:**

`docker inspect` provides detailed information about a Docker image or container, including its configuration and metadata.

**🔍 Why use it?**

To understand how an image or container is configured.

### 📌 Understanding the `Cmd` Field

**📤 Example Output:**

```json
"Cmd": [
    "/bin/bash"
]
```

**💡 Meaning:**

`Cmd` specifies the default command configured for the image.

In this example, the default command is `/bin/bash`.

When a container is started without overriding its default command, Docker uses this configured command.

---

## 10. 📜 Docker Logs

### 📝 Command Syntax

```bash
docker logs <container_id>
```

**📖 Definition:**

The `docker logs` command displays the standard output (stdout) and standard error (stderr) generated by a container's main process.

**🔍 Why use it?**

- 📋 To check application output.
- ⚠️ To identify errors.
- 🔎 To understand what the container's main process is reporting.

### 💡 Example

```bash
docker logs abc123
```

Here, `abc123` represents the container ID.

Docker displays the available logs generated by the container's main process.

---

## 📌 Quick Command Reference

| Command | Purpose |
|---|---|
| 📥 `docker pull ubuntu` | Download Ubuntu image |
| 📋 `docker images` | List local images |
| 📥 `docker pull amazonlinux` | Download Amazon Linux image |
| 🖥️ `docker version` | Check Docker version |
| 🚀 `docker run ubuntu` | Create and start container |
| 🔀 `docker run -dt ubuntu` | Detached mode |
| 💻 `docker run -it ubuntu` | Interactive mode |
| 🔑 `docker exec -it` | Access existing container |
| ⚙️ `ps aux` | List processes |
| 🔢 `ps aux \| wc -l` | Count process lines |
| 📋 `docker ps` | List running containers |
| 📦 `docker ps -a` | List all containers |
| 🔴 `docker ps -a \| grep Exited` | Filter exited containers |
| 🔎 `docker inspect ubuntu` | Inspect image/container |
| 📜 `docker logs <container_id>` | View container logs |

---

## 🎯 Learning Outcomes

After completing Day 02, I understood:

- 🌐 Docker Hub and local image management.
- 📥 Downloading and listing Docker images.
- 📦 Difference between Docker Images and Containers.
- 🚀 Creating containers using `docker run`.
- 🔀 Detached and Interactive terminal modes.
- 🔑 Accessing existing containers using `docker exec`.
- ⚙️ Understanding Linux processes.
- 🔍 Managing and listing Docker containers.
- 🛠️ Inspecting Docker configurations.
- 📜 Viewing container logs.

---

## 🤝 Let's Connect

👨‍💻 **GitHub:** [@byteAlok](https://github.com/byteAlok)

🌐 **Portfolio:** [aloksde.com](https://aloksde.com)

⭐ If you find these notes helpful, consider giving this repository a Star. It motivates me to keep learning, documenting, and sharing practical knowledge.

**Thanks for visiting! Happy Learning & Keep Building! 🚀**
