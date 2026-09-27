Breakdown of the task requirements and how to approach them step-by-step for this lab environment.

---

### 1. Understanding the Lab Layout

* **Task Description (Left Panel):** Outlines your goal:
1. Install `docker-ce` and `docker-compose-plugin` (or `docker-compose`) on **App Server 1**.
2. Start and enable the `docker` service on **App Server 1**.


* **Terminal 1 (Right Panel):** You start logged in as user `thor` on the **`jump-host`** machine. The task target is **App Server 1** (`stapp01`), not the jump host itself.

---

### 2. Connect to App Server 1

From `jump-host`, SSH into `stapp01` (App Server 1). Check **Details of all Users and Servers** at the top right if you need credentials, but typically you can SSH directly:

```bash
ssh tony@stapp01

```

*(Password can be found in the server details panel, commonly `Ir0nM@n`).*

---

### 3. Install Docker CE & Docker Compose

Once inside `stapp01`, escalate to root or use `sudo` to set up the repository and install packages:

```bash
# 1. Install prerequisites
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

# 2. Add the official Docker repository
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 3. Install docker-ce and docker-compose plugin/package
sudo yum install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

```

*Note: If the lab specifically checks for the standalone `docker-compose` package rather than the plugin, run:*

```bash
sudo yum install -y docker-ce docker-compose

```

---

### 4. Start and Enable Docker Service

Start the Docker service and enable it so it launches automatically on boot:

```bash
sudo systemctl start docker
sudo systemctl enable docker

```

---

### 5. Verify Installation

Ensure Docker is running and working properly:

```bash
# Check service status
sudo systemctl status docker

# Verify Docker version
docker --version

```

---

Breakdown of what Docker and Docker Compose are, why you are performing these specific steps, and how it all connects to your goal.

---

### 1. What are Docker CE and Docker Compose?

* **Docker CE (Community Edition):** The core containerization platform. Docker CE (Community Edition) is a free, open-source containerization platform used to build, test, and run applications inside isolated environments called containers. It allows you to package an application along with all its dependencies (libraries, code, configurations) into a standardized unit called a **container**. Containers run isolated from each other and the host OS, ensuring your application works identically across development, testing, and production environments.

  ## Core Components
  
  * Docker Daemon (dockerd): The background service that manages Docker containers, images, networks, and storage.
  * Docker CLI (docker-ce-cli): The command-line interface used by operators and developers to send commands to the daemon.
  * Containerd (containerd.io): The underlying industry-standard container runtime that executes container lifecycles.


* **Docker Compose:** A tool for defining and running multi-container Docker applications using a single YAML configuration file (`docker-compose.yml`). Instead of starting multiple containers manually with long terminal commands, Compose lets you launch and configure an entire stack (e.g., a web server, API, and database) with a single command: `docker compose up`.

---

### 2. Step-by-Step Breakdown: What Each Command Does & Why

Here is why each step in the setup process was necessary and what happens behind the scenes:

#### Step A: Installing Prerequisites (`yum-utils`, `device-mapper-persistent-data`, `lvm2`)

* **What it means:** Installs lower-level Linux utilities for repository management and storage drivers.
* **Why we do it:** Docker needs `yum-config-manager` (provided by `yum-utils`) to add new software repositories, and requires device-mapper capabilities to efficiently manage container filesystems and storage layers.

#### Step B: Adding the Official Docker Repository

* **What it means:** Configures `yum` (the package manager) to fetch Docker directly from Docker's official distribution servers rather than default OS repositories, which often contain outdated versions.
* **Why we do it:** Ensures you get the latest stable releases of `docker-ce` and `docker-compose-plugin`.

#### Step C: Installing Docker CE & Docker Compose (`docker-ce`, `docker-ce-cli`, `containerd.io`, `docker-compose-plugin`)

* **What it means:**
* `docker-ce`: The core background daemon (`dockerd`) that manages containers, images, networks, and volumes.
* `docker-ce-cli`: The command-line tool that lets you type `docker` commands in the terminal.
* `containerd.io`: The underlying container runtime that actually spawns and executes containers.
* `docker-compose-plugin`: Adds the `docker compose` subcommand to manage multi-container setups.


* **Why we do it:** Fulfills Requirement #1 of the task by equipping App Server 1 with the full suite of software needed to build and run containerized apps.

#### Step D: Starting and Enabling the Docker Service (`systemctl start/enable docker`)

* **What it means:**
* `systemctl start docker`: Immediately boots up the Docker background process (`dockerd`).
* `systemctl enable docker`: Configures Linux to start the Docker service automatically whenever the server boots or reboots.


* **Why we do it:** Fulfills Requirement #2 of the task. Installing packages only places the files on disk; without starting the background service, running any `docker` command will result in a connection error (`Cannot connect to the Docker daemon`).

---

### 3. What is the Big Picture Goal?

The overall goal for the DevOps team is to **transition traditional application deployments into containerized workloads**:

1. **Environment Consistency:** Eliminates the classic "it works on my machine" problem by bundling the application and its environment together.
2. **Simplified Testing & Deployment:** Instead of manually configuring databases, web servers, and runtime dependencies on every server, the team can spin up test environments instantly using Docker containers and Docker Compose scripts.
3. **Infrastructure Readiness:** Preparing **App Server 1** (`stapp01`) with Docker ensures it is ready to host and test containerized components for Project Nautilus.

Once confirmed, switch back to the lab window and click **Check** to submit your work.
