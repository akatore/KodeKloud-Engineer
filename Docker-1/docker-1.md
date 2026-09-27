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

Once confirmed, switch back to the lab window and click **Check** to submit your work.
