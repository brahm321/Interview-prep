
If a project has a tech stack with different configurations, instead of someone else setting it up step by step, Docker can package all those configurations together.
This makes the setup reproducible and easy to run anywhere.

2. Virtualization (Before Docker)
Earlier, this problem was solved using virtualization.
On a physical machine (hardware), you first install an Operating System (Host OS).
On top of the Host OS, you run a hypervisor (like VMware, VirtualBox, Hyper-V).
The hypervisor creates Virtual Machines (VMs). Each VM has:
its own Guest OS
virtualized CPU, RAM, and disk (treated like hardware).
Inside the VM, you set up your application environment.
You could then create an image (snapshot) of the VM and share it.
Others could run that VM image to reproduce the same environment.


**3. Problem with Virtualization**

- Each Virtual Machine (VM) needs its **own full Guest Operating System**.
    
- This makes VMs **heavyweight**:
    
    - They consume a lot of **disk space** (several GBs).
        
    - They need significant **CPU and memory** to run.
        
- Booting up a VM is **slow** because an entire OS has to start.
    
- Running multiple VMs on one machine is inefficient — resources are wasted since each VM duplicates the OS.
    
- Sharing VM images is possible, but they are **large and slow to transfer**.


**4. Core Components of Docker**

* **Docker Engine**

  * The runtime that allows you to create, run, and manage containers.
  * It includes the **Docker daemon** (runs in the background), the **REST API**, and the **CLI** (`docker` commands).

* **Containers**

  * A container is a **lightweight, isolated runtime environment** for an application.
  * It includes the application code + all dependencies it needs (libraries, configs), but **shares the Host OS kernel** instead of running a full OS.
  * This makes containers start quickly and use fewer resources than VMs.

* **Images**

  * An image is a **read-only blueprint** for creating containers.
  * It contains everything needed to run an app: code, runtime, libraries, environment variables, etc.
  * Instead of sharing a running container, teams share its **image** (lightweight, layered, and portable).
  * From an image, Docker can spin up any number of containers.

* **Dockerfile**

  * A simple **text file with instructions** to build a Docker image.
  * Example: which base image to use, what dependencies to install, what command to run when the container starts.
  * Automates image creation instead of building manually.

* **Docker Hub**

  * A **cloud registry** where Docker images are stored and shared.
  * Public images (like `nginx`, `mysql`) are available, and private repos can also be hosted.

* **Volumes**

  * Mechanism to **store data permanently** outside of a container.
  * Since containers are temporary (deleted when stopped/removed), volumes keep important data (like database files, logs).

* **Docker Compose**

  * A tool to define and run **multi-container applications**.
  * Uses a `docker-compose.yml` file to specify multiple services (like app + database + cache).
  * Runs them together with one command (`docker-compose up`).


---

### 📝 Docker Notes (Rectified)

**5. Basic Docker Commands (Lifecycle)**

1. **Search for an image**

   ```bash
   docker search hello-world
   ```

   * Looks up images from Docker Hub registry.

2. **Create a container (only creates, does not start)**

   ```bash
   docker create hello-world
   ```

   * Sets up a container from the image but leaves it in “created” state.
   * At this point, container exists but isn’t running.

3. **Run a container (create + start in one step)**

   ```bash
   docker run hello-world
   ```

   * If image not found locally → pulls from Docker Hub.
   * Creates a container and **starts it immediately**.
   * Most commonly used command.

4. **List images**

   ```bash
   docker images
   ```

   * Shows all downloaded images on your machine.

5. **List containers**

   * Running containers:

   ```bash
     docker ps
     ```
   * All containers (running + stopped):

  ```bash
     docker ps -a
     ✅ (your guess was right!)  
    ```

6. **Pause / Unpause a container**

   ```bash
   docker pause <container-id>
   docker unpause <container-id>
   ```

   * Suspends processes in a container, then resumes them.

7. **Remove a container**

   ```bash
   docker rm <container-id>
   ```

   * Deletes container (must be stopped first).

8. **Remove an image**

   ```bash
   docker rmi <image-id>
   ```

   * Deletes the image from local storage.

Perfect 👌 you’ve got the overall picture — let me refine and structure it properly:

---


* **Docker Client**

  * The interface through which users interact with Docker.
  * Commands like `docker run`, `docker pull`, `docker ps` are given here.
  * Client communicates with the **Docker Daemon** via REST API.

* **Docker Daemon (`dockerd`)**

  * The **core service** that does the actual work.
  * Listens for requests from the client (via API).
  * Responsible for creating, running, stopping, pulling images, managing networks, volumes, etc.
  * Example: when you run `docker run hello-world`, the **client sends request → daemon pulls image → daemon creates and starts container**.

* **Docker Objects**

  * Managed by the Daemon:

    * **Images** → blueprints for containers.
    * **Containers** → running instances of images.
    * **Volumes** → persistent storage for containers.
    * **Networks** → communication between containers or with the outside world.

* **Registry**

  * Storage for Docker images.
  * Can be **Public (Docker Hub)** or **Private registry**.
  * When `docker pull` is used, Daemon fetches the image from a registry.
  * When `docker push` is used, Daemon uploads the image to a registry.

---

* Requests **don’t go directly from client → registry**.
* Instead: **Client → Daemon → Registry (if needed)**.
* Daemon is the “brain” of Docker; client is just a messenger.

Excellent 👌 you’ve just walked through a **real deployment flow** inside Docker. Let me carefully **rectify, reorder, and explain why each step matters** so you can see the right process.

---

### 📝 Docker Notes (Rectified)

**8. Running a Spring Boot App inside Docker (manual method)**

1. **Start a base container (OpenJDK)**

   ```bash
   docker run -it openjdk:26-trixie
   ```

   * `-it` → interactive terminal, so it keeps running.
   * Default command here is `jshell`, so the container keeps running in shell mode.

2. **Explore the container**

   ```bash
   docker exec <container_name> ls -a
   docker exec <container_name> ls /tmp
   ```

   * `docker exec` runs a command inside a running container.
   * Verified `/tmp` directory exists.

3. **Copy Spring Boot JAR into container**

   ```bash
   docker cp target/rest.jar <container_name>:/tmp
   ```

   * `docker cp` copies files from host → container.
   * Now `/tmp/rest.jar` is inside the container.

4. **Commit the container into a new image**

   ```bash
   docker commit <container_name> my_personal_image:v1
   ```

   * Saves current state of container (with JAR inside) as a new image.

5. **Problem**: Default command in `openjdk` image is `jshell`.

   * So running `docker run my_personal_image:v1` → starts `jshell`, not your app.

6. **Fix default command using `--change`**

   ```bash
   docker container commit --change='CMD java -jar /tmp/app-name.jar' <container_name> <docker_registry>/app-name:
   ```

   * Overrides default startup command to run your Spring Boot JAR.

7. **Expose ports when running**

   ```bash
   docker run -p 8080:8080 my_personal_image:v2
   ```

   * Maps container’s `8080` port → host’s `8080`.
   * Now the Spring Boot app is accessible at `http://localhost:8080`.

### 📝 Docker Notes (Rectified)

**9. Running Spring Boot App with a Dockerfile (best practice)**

**Dockerfile**

```dockerfile
FROM openjdk:26-jdk   # Base image with Java runtime
ADD target/rest.jar rest.jar   # Copies jar into container
ENTRYPOINT ["java", "-jar", "/rest.jar"]   # Run the jar when container starts
```

---

**Explanation**

* **`FROM openjdk:26-jdk`**

  * Chooses base image (has Java already installed).
  * You used `openjdk:asd` — but that tag doesn’t exist, so it should be something valid like `openjdk:26-jdk` or `openjdk:17-jdk`.

* **`ADD target/rest.jar rest.jar`**

  * Copies `target/rest.jar` from **host machine → container filesystem**.
  * By default, files are copied into the **working directory** inside the container (which is `/` unless you set `WORKDIR`).
  * So this places your JAR at `/rest.jar`.
  * That’s why `/tmp` is **not needed** here — you control the exact destination with this line.
  * Alternative (more common) is `COPY` instead of `ADD` (both work for simple file copy).

* **`ENTRYPOINT ["java", "-jar", "/rest.jar"]`**

  * Defines the default command to run when container starts.
  * Uses **absolute path `/rest.jar`** because file is placed at root (`/`) by the `ADD` instruction.
  * Now when you run the container, it automatically runs your Spring Boot app.

---

**Build the image**

```bash
docker build -t brahmesh:v3 .
```

* `-t brahmesh:v3` → tags image.
* `.` → build context is the current directory (Dockerfile + target folder must be here).

**Run the container with port mapping**

```bash
docker run -p 8080:8080 brahmesh:v3
```

* Maps container’s port 8080 → host’s port 8080.
* Now app accessible at: `http://localhost:8080`.



