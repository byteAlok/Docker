# 🐳 Day 04 – Dockerfile, Image Build & Container Execution

> 🚀 Learning how to create Dockerfiles, build custom Docker images, understand Docker instructions, and execute containers.

---

## 📚 Topics Covered

- 📄 Dockerfile Introduction
- 🔤 Dockerfile Instructions & Case Sensitivity
- 🖼️ FROM – Base Image
- 📂 WORKDIR – Working Directory
- 📋 COPY – Copy Files into Image
- ⚙️ RUN – Execute Build Commands
- 🚀 CMD – Default Container Command
- 🔀 Executing Multiple Commands using CMD
- 🏗️ Docker Image Build Process
- 🏷️ Docker Image Tagging (`-t`)
- 📦 Default & Custom Dockerfile Names
- 📂 Build Context (`.`)
- 🔍 Container Troubleshooting
- 🔎 Inspect Container Details (`docker inspect`)
- 📜 Check Container Logs (`docker logs`)
- 🌐 Check Listening Network Ports (`ss -tuln`)
- 🖥️ Base OS vs Full OS
- 🐧 Linux Containers vs Windows Containers

---

## 1. 📄 Dockerfile Introduction

**📖 Definition:**

A Dockerfile is a plain-text configuration file that contains a sequential list of instructions used by Docker to automatically build a Docker image.

It defines the base environment, working directory, application files, dependencies, and default command required by an application.

**💡 Why use Dockerfile?**

- 📦 Automates Docker image creation.
- ⚙️ Defines application dependencies.
- 🖥️ Provides a consistent application environment.
- 🔄 Makes image building repeatable.

### 🔤 Dockerfile Case Sensitivity

Dockerfile instructions are conventionally written in uppercase.

**Example:**

```dockerfile
FROM ubuntu
WORKDIR /app
COPY . /app
RUN apt-get update
CMD ["python3", "--version"]
```

**Important:**

- Dockerfile instructions are conventionally written in uppercase.
- Instruction keywords are not case-sensitive.
- Filenames on Linux are case-sensitive.
- `Dockerfile` is the default filename used by `docker build`.

---

## 2. 🖼️ FROM Instruction – Base Image

**📖 Definition:**

`FROM` specifies the base image from which a new Docker image will be built.

It is generally the first instruction in a Dockerfile.

**💡 Why use it?**

Every Docker image needs a starting environment. `FROM` specifies that starting image.

### 📝 Syntax

```dockerfile
FROM image_name
```

### 💻 Example

```dockerfile
FROM ubuntu
```

**Explanation:**

- `FROM` → Dockerfile instruction.
- `ubuntu` → Base image.

Docker uses the Ubuntu image as the starting point for building our custom image.

**📌 Understanding:**

```text
       🖼️ Ubuntu Image
             |
             ▼
       📄 Custom Dockerfile
             |
             ▼
        📦 Custom Image
```

---

## 3. 📂 WORKDIR Instruction – Working Directory

**📖 Definition:**

`WORKDIR` sets the working directory for subsequent Dockerfile instructions such as `RUN`, `COPY`, and `CMD`.

If the specified directory does not exist, Docker creates it.

**💡 Why use it?**

It provides a fixed location inside the image where application files and commands can be organized.

### 📝 Syntax

```dockerfile
WORKDIR /directory_name
```

### 💻 Example

```dockerfile
FROM ubuntu

WORKDIR /app
```

**Explanation:**

- `FROM ubuntu` → Selects Ubuntu as the base image.
- `WORKDIR /app` → Sets `/app` as the working directory.

**📂 Resulting Structure:**

```text
/
└── app/
```

The subsequent instructions will use `/app` as their working directory unless it is changed.

---

## 4. 📋 COPY Instruction – Copy Files

**📖 Definition:**

`COPY` copies files and directories from the build context into the Docker image filesystem.

**💡 Why use it?**

To copy application source code, configuration files, and other required files into the image.

### 📝 Syntax

```dockerfile
COPY source destination
```

### 💻 Example

```dockerfile
FROM ubuntu

WORKDIR /app

COPY . /app
```

**Explanation:**

| Part | Meaning |
|---|---|
| `COPY` | Dockerfile instruction |
| `.` | Source: contents of the build context |
| `/app` | Destination inside the image |

**📂 Local Project Structure:**

```text
my-project/
│
├── Dockerfile
├── app.py
├── requirements.txt
└── config/
    └── settings.py
```

**📦 After COPY:**

```text
Container Image
│
└── app/
    │
    ├── app.py
    ├── requirements.txt
    └── config/
        └── settings.py
```

The files from the build context are copied into `/app`.

---

## 5. ⚙️ RUN Instruction – Execute Commands

**📖 Definition:**

`RUN` executes commands during the Docker image build process.

The changes produced by these commands become part of the resulting image.

**💡 Why use it?**

To install software, update packages, and configure the application environment while building an image.

### 📝 Syntax

```dockerfile
RUN command
```

### 💻 Example 1: Update Ubuntu Packages

```dockerfile
FROM ubuntu

RUN apt update -y
```

**Explanation:**

- `RUN` → Executes a command during image build.
- `apt update -y` → Updates the package index.

### 💻 Example 2: Install Python & PIP

```dockerfile
FROM ubuntu

RUN apt-get update && apt-get install -y python3 python3-pip
```

**Command Breakdown:**

| Part | Description |
|---|---|
| `RUN` | Executes command during image build |
| `apt-get update` | Updates package lists |
| `&&` | Executes next command if previous command succeeds |
| `apt-get install` | Installs packages |
| `-y` | Automatically confirms installation |
| `python3` | Python interpreter |
| `python3-pip` | Python package installer |

**📌 Why combine commands?**

Both commands are executed in a single `RUN` instruction, which avoids creating a separate image layer for the package-index update.

**💡 Remember:**

`RUN` executes during image building, not when the container starts.

---

## 6. 🚀 CMD Instruction – Default Container Command

**📖 Definition:**

`CMD` specifies the default command or arguments that Docker uses when starting a container.

It is generally used to define the application or process that should run when the container starts.

**💡 Why use it?**

To specify what should execute when someone starts a container from the image without overriding its default command.

### 📝 Syntax

```dockerfile
CMD ["executable", "argument"]
```

### 💻 Example

```dockerfile
FROM ubuntu

RUN apt-get update && apt-get install -y python3

CMD ["python3", "--version"]
```

**Explanation:**

- `CMD` → Defines the default container command.
- `python3` → Python interpreter.
- `--version` → Displays the Python version.

**📤 Expected Output:**

```text
Python 3.x.x
```

The exact version depends on the installed Ubuntu package.

### 🔀 Executing Multiple Commands Using CMD

If we want to execute multiple commands through `CMD`, we can use `bash -c`.

**📝 Syntax:**

```dockerfile
CMD ["bash", "-c", "command1 && command2 && command3"]
```

**💻 Example:**

```dockerfile
FROM ubuntu

RUN apt-get update && apt-get install -y python3

CMD ["bash", "-c", "echo Hello Docker && python3 --version && echo Container Started"]
```

**Command Breakdown:**

| Part | Description |
|---|---|
| `CMD` | Defines the default command when the container starts |
| `bash` | Starts the Bash shell |
| `-c` | Tells Bash to execute the command string that follows |
| `echo Hello Docker` | Prints a message |
| `&&` | Executes the next command only if the previous command succeeds |
| `python3 --version` | Displays the installed Python version |
| `echo Container Started` | Prints another message |

**📤 Expected Output:**

```text
Hello Docker
Python 3.x.x
Container Started
```

The exact Python version may differ depending on the installed package.

**💡 Why use `bash -c`?**

The exec form of `CMD` accepts an executable and its arguments. It does not directly interpret shell operators such as `&&`.

By using `bash -c`, we ask Bash to interpret and execute multiple commands from one command string.

**⚠️ Important:**

- Multiple shell commands can be written inside one `CMD` using `bash -c`.
- We can connect commands using `&&`.
- `&&` runs the next command only if the previous command succeeds.
- A Dockerfile can contain multiple `CMD` instructions, but only the last `CMD` instruction takes effect.
- `CMD` defines the default command; it can be overridden when a command is supplied to `docker run`.

---

## 7. ⚖️ RUN vs CMD

Both instructions execute commands, but they work at different stages.

| RUN | CMD |
|---|---|
| Executes during image build | Executes when container starts |
| Used to install packages | Used to define default command |
| Changes become part of image | Defines container startup behavior |
| Can appear multiple times | Only the last CMD instruction takes effect |

**💡 Remember:**

- ⚙️ `RUN` → Build Time.
- 🚀 `CMD` → Container Start Time.

---

## 8. 🏗️ Docker Image Build Process

After writing a Dockerfile, we use `docker build` to create a Docker image.

### 📌 Command Syntax

```bash
docker build -t img1 .
```

**Command Breakdown:**

| Part | Description |
|---|---|
| `docker` | Docker CLI |
| `build` | Builds a Docker image |
| `-t` | Assigns image name/tag |
| `img1` | Image name |
| `.` | Current directory as build context |

### 🔍 Understanding the Build Process

```text
       📂 Project Directory
               |
               ▼
           📄 Dockerfile
               |
               ▼
          🏗️ docker build
               |
               ▼
           📦 Docker Image
               |
               ▼
           🚀 Container
```

### 📤 Example Output

```text
[+] Building ...
 => [internal] load build definition
 => [internal] load metadata
 => [1/3] FROM ubuntu
 => [2/3] WORKDIR /app
 => [3/3] RUN apt-get update
 => exporting to image
 => naming to docker.io/library/img1
```

Actual build output may vary.

### 📋 Verify Created Image

```bash
docker images
```

**Example Output:**

```text
REPOSITORY   TAG       IMAGE ID       SIZE
img1         latest    abc123         ...
```

---

## 9. 📄 Default Dockerfile vs Custom Filename

Docker uses `Dockerfile` as its default build instruction filename.

However, we can also use a custom filename without any extension.

### 🅰️ Approach 1: Default Dockerfile

**📂 Project Structure:**

```text
my-project/
│
├── Dockerfile
└── app.py
```

**📄 Dockerfile Content:**

```dockerfile
FROM ubuntu

WORKDIR /app

COPY . /app

RUN apt-get update && apt-get install -y python3 python3-pip

CMD ["python3", "--version"]
```

**🏗️ Build Command:**

```bash
docker build -t img1 .
```

**Explanation:**

- `-t img1` → Assigns image name `img1`.
- `.` → Uses the current directory as build context.
- Docker automatically searches for a file named `Dockerfile`.

### 🅱️ Approach 2: Custom Dockerfile Name

We can give our Dockerfile any suitable filename.

**📂 Project Structure:**

```text
my-project/
│
├── MyDockerFile
└── app.py
```

**📄 MyDockerFile Content:**

```dockerfile
FROM ubuntu

WORKDIR /app

COPY . /app

RUN apt-get update && apt-get install -y python3 python3-pip

CMD ["python3", "--version"]
```

**🏗️ Build Command:**

```bash
docker build -t img1 -f MyDockerFile .
```

**Explanation:**

| Part | Description |
|---|---|
| `docker build` | Builds an image |
| `-t img1` | Assigns image name |
| `-f` | Specifies the Dockerfile |
| `MyDockerFile` | Custom Dockerfile filename |
| `.` | Current directory as build context |

**💡 Important:** When the filename is different from the default `Dockerfile`, use `-f` to specify it.

---

## 10. 📂 Understanding Build Context (.)

**📖 Definition:**

Build Context is the collection of files and directories that Docker can access during the image build process.

It is provided as the final argument of the `docker build` command.

**💡 Why use it?**

Docker needs access to application files and other resources when executing instructions such as `COPY`.

### 📂 Example Project Structure

```text
my-project/
│
├── Dockerfile
├── app.py
├── requirements.txt
│
└── config/
    └── settings.py
```

### 🏗️ Build Command

```bash
docker build -t img1 .
```

Here, `.` represents the current directory (`my-project`) as the build context.

### 🔍 Build Context vs Dockerfile

| Dockerfile | Build Context |
|---|---|
| Contains build instructions | Contains files available to the build |
| Defines how the image is built | Defines which files Docker can access |
| Selected using `-f` | Specified as the final argument |

### 💻 Default Dockerfile + Build Context

```bash
docker build -t img1 .
```

- Default Dockerfile is used.
- Current directory is the build context.

### 💻 Custom Dockerfile + Build Context

```bash
docker build -t img1 -f MyDockerFile .
```

- `-f MyDockerFile` → Selects custom Dockerfile.
- `.` → Uses current directory as build context.

**📌 Important:** Dockerfile location and build context location are two different things. Changing the Dockerfile filename does not automatically change the build context.

**💡 Remember:**

- `-f` → Which Dockerfile?
- `.` → Which files are available for the build?
- `-t` → What should the image be named?

---

## 11. 🔍 Container Troubleshooting

When a container exits or an application does not behave as expected, we can use Docker and Linux commands to investigate.

### 📋 Check All Containers

```bash
docker ps -a
```

**Definition:** Displays all containers, including running and stopped containers.

**Why use it?**

To identify whether a container is running or has exited.

**Important columns:**

- `CONTAINER ID` – Unique container ID.
- `IMAGE` – Image used to create the container.
- `COMMAND` – Main command associated with the container.
- `STATUS` – Current container status.
- `PORTS` – Published or exposed port information.
- `NAMES` – Container name.

### 🔎 Inspect Container Details

```bash
docker inspect <container_id>
```

**Definition:** Displays detailed information about a Docker container in JSON format.

**Why use it?**

To investigate the container's configuration and runtime details.

It helps us check:

- **Container State** – Whether the container is running or stopped.
- **Exit Code** – The exit status of the container's main process.
- **Image** – The image used to create the container.
- **Command** – The configured command.
- **Network Settings** – Container IP address and network information.
- **Port Bindings** – Host and container port mapping.
- **Mounts** – Mounted volumes and directories.

**💻 Example:**

```bash
docker inspect <container_id>
```

This command displays detailed information about the specified container in JSON format.

### 🎯 Inspect a Specific Property

We can use the `--format` option to display a specific property instead of the complete JSON output.

**Example – Check Container Status:**

```bash
docker inspect --format='{{.State.Status}}' <container_id>
```

**Command Breakdown:**

| Part | Description |
|---|---|
| `docker inspect` | Inspects a Docker object |
| `--format` | Displays selected information |
| `{{.State.Status}}` | Reads the container's state status |
| `<container_id>` | ID of the container to inspect |

**📤 Possible Output:**

```text
running
```

Or:

```text
exited
```

**💡 Why use it?**

It helps us quickly check whether a particular container is running or has exited without reading the complete inspection output.

### 📜 Check Container Logs

```bash
docker logs <container_id>
```

**Definition:** Displays the standard output and standard error generated by the container's main process.

**Why use it?**

To understand application output and identify errors.

It can help us identify:

- Application errors.
- Startup failures.
- Missing dependencies.
- Messages printed by the application.

**💻 Example:**

```bash
docker logs <container_id>
```

### 🌐 Check Listening Network Ports

```bash
ss -tuln
```

**Definition:** Displays listening TCP and UDP sockets using numeric addresses and port numbers.

**Why use it?**

To check which ports are listening on the Linux system.

It helps us investigate whether a service is listening on the expected network port.

**🔍 Command Breakdown:**

| Part | Meaning |
|---|---|
| `ss` | Socket statistics command |
| `-t` | Displays TCP sockets |
| `-u` | Displays UDP sockets |
| `-l` | Displays listening sockets |
| `-n` | Displays numeric addresses and port numbers instead of resolving names |

**📤 Example Output:**

```text
Netid  State   Recv-Q  Send-Q  Local Address:Port  Peer Address:Port
tcp    LISTEN  0       128     0.0.0.0:80          0.0.0.0:*
tcp    LISTEN  0       128     0.0.0.0:22          0.0.0.0:*
```

**Explanation:**

- `LISTEN` → The service is listening for incoming connections.
- `0.0.0.0:80` → Port 80 is listening on all IPv4 interfaces.
- `0.0.0.0:22` → Port 22 is listening on all IPv4 interfaces.

**📌 Important:**

- Run `ss -tuln` on the Linux server to check its listening ports.
- To check listening ports inside a container, execute the command inside that container.
- Minimal container images may not have the `ss` command installed.
- On Ubuntu, the `ss` command is provided by the `iproute2` package.

**💡 Remember:**

| Command | Purpose |
|---|---|
| `docker ps -a` | Checks all containers |
| `docker inspect <container_id>` | Checks detailed container configuration |
| `docker logs <container_id>` | Checks container application output and errors |
| `ss -tuln` | Checks listening TCP and UDP ports |

---

## 12. 🖥️ Base OS vs Full OS

### 🖼️ Base OS

A container image generally provides an operating-system userspace environment, including utilities, libraries, and applications.

It does not normally contain or boot its own independent kernel.

### 💻 Full OS

A traditional full operating system environment includes:

- 🧠 Kernel
- 📂 Filesystem
- ⚙️ System utilities
- 📦 Libraries and applications

### ⚖️ Main Difference

| Base OS Container | Full OS / Virtual Machine |
|---|---|
| Shares the host kernel | Has its own guest kernel |
| Provides isolated userspace | Provides a complete guest OS environment |
| Lightweight | Generally heavier |
| Starts container processes | Boots a guest operating system |

**💡 Important:** Linux containers share the host Linux kernel. Running Linux containers on Windows commonly involves a Linux VM or WSL2 backend.

### 🐧 Linux Containers vs Windows Containers

Docker containers share the kernel of the operating system environment on which they run.

This means Linux containers require a compatible Linux kernel, while Windows containers require a compatible Windows kernel.

**📌 Linux Server:**

- A Linux server has a Linux kernel.
- It can run Linux-based containers such as Ubuntu, Nginx, and Python containers.
- It cannot natively run Windows OS containers because Windows containers require a Windows kernel.

**📌 Windows Server:**

- A Windows server has a Windows kernel.
- It can run compatible Windows containers.
- Linux containers on Windows commonly use a Linux VM or WSL2 backend, depending on the Docker environment.

### ⚖️ Linux Container vs Windows Container

| Linux Container | Windows Container |
|---|---|
| Requires a compatible Linux kernel | Requires a compatible Windows kernel |
| Can run on a Linux server | Requires a Windows container host |
| Shares the Linux host kernel | Shares the Windows host kernel |
| Uses Linux-based container images | Uses Windows-based container images |

**💡 Example:**

Suppose we have an Ubuntu Linux server.

We can run:

```text
🐧 Ubuntu Server
       |
       ├── 🐳 Ubuntu Container
       |
       ├── 🐳 Nginx Container
       |
       └── 🐳 Python Container
```

But we cannot directly run a Windows OS container on this Linux server because it does not have a Windows kernel.

**⚠️ Important Clarification:**

This limitation is specifically about running Windows containers. It does not mean Windows applications can never run on Linux. The operating-system kernel requirement applies to Windows containers.

---

## 📌 Quick Command Reference

| Command | Purpose |
|---|---|
| `docker build -t img1 .` | Build image using default Dockerfile |
| `docker build -t img1 -f MyDockerFile .` | Build using custom Dockerfile |
| `docker images` | List Docker images |
| `docker ps -a` | List all containers |
| `docker inspect <container_id>` | View detailed container information |
| `docker inspect --format='{{.State.Status}}' <container_id>` | Check container status |
| `docker logs <container_id>` | View container logs |
| `ss -tuln` | Check listening TCP and UDP ports |

---

## 🎯 Learning Outcomes

After completing Day 04, I understood:

- 📄 What a Dockerfile is and why it is used.
- 🔤 Dockerfile instruction conventions and case sensitivity.
- 🖼️ How to select a base image using `FROM`.
- 📂 How to set a working directory using `WORKDIR`.
- 📋 How to copy files using `COPY`.
- ⚙️ How to execute build-time commands using `RUN`.
- 🚀 How to define container startup commands using `CMD`.
- 🔀 How to execute multiple commands using `CMD` and `bash -c`.
- ⚖️ Difference between `RUN` and `CMD`.
- 🏗️ How to build Docker images using `docker build`.
- 🏷️ How to assign image names using `-t`.
- 📄 Difference between default and custom Dockerfile filenames.
- 📂 What Build Context means and why it is required.
- 🔍 How to troubleshoot containers using `docker ps -a`.
- 🔎 How to inspect container details using `docker inspect`.
- 📜 How to view container logs using `docker logs`.
- 🌐 How to check listening ports using `ss -tuln`.
- 🖥️ Difference between container userspace and a full operating system.
- 🐧 Difference between Linux and Windows containers.
- 🔑 Why Linux servers cannot natively run Windows containers.

---

## 🤝 Let's Connect

👨‍💻 **GitHub:** [@byteAlok](https://github.com/byteAlok)

🌐 **Portfolio:** [aloksde.com](https://aloksde.com)

⭐ If you find these notes helpful, consider giving this repository a Star. It motivates me to keep learning, documenting, and sharing practical knowledge.

**Thanks for visiting! Happy Learning & Keep Building! 🚀**
