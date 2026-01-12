### Step 1: Create a Dockerfile

Create a file named **`Dockerfile`** in the root directory of your project.

### Step 2: Define the Dockerfile content

```docker
FROM eclipse-temurin:17
LABEL maintainer="chrisannlee97@gmail.com"

WORKDIR /app

COPY target/springboot-docker-demo-0.0.1-SNAPSHOT.jar /app/springboot-docker-demo.jar

ENTRYPOINT ["java", "-jar", "springboot-docker-demo.jar"]
```

Explanation of Commands

- **FROM**
    
    Specifies the base image (Java 17 using Eclipse Temurin).
    
- **LABEL**
    
    Adds metadata such as the maintainer’s information.
    
- **WORKDIR**
    
    Sets the working directory inside the container.
    
- **COPY**
    
    Copies the Spring Boot JAR file into the container.
    
- **ENTRYPOINT**
    
    Defines the command that runs when the container starts.
    

**Build Application Jar File**

![Screenshot 2026-01-02 171135.png](attachment:ba3e5afa-d4a0-4a44-adba-52d902938f38:Screenshot_2026-01-02_171135.png)

### Step 3: Build Docker Image from Dockerfile

Run the following command in the project root directory (where the `Dockerfile` is located):

```bash
docker build -t springboot-docker-demo .
```

Explanation

- **docker build**: Builds a Docker image
- **-t springboot-docker-demo**: Tags (names) the image
- **.**: Uses the current directory as the build context

### Step 4: Run Docker Image

Run the container from the Docker image using:

```bash
docker run -p 8080:8080 springboot-docker-demo
```

Explanation

- **docker run**: Starts a new container from an image
- **-p 8080:8080**: Maps port 8080 of the container to port 8080 on your machine
- **springboot-docker-demo**: The name of the Docker image to run

### **Extra Note:** Run Docker Image in background

**1. Run a Docker container**

```powershell
docker run -p 8081:8080 -d springboot-docker-demo
```

Explanation:

- **docker run** → Starts a new container from an image.
- **-p 8081:8080** → Maps port **8080** inside the container to **8081** on your host machine.
    - This means you can access the app at `http://localhost:8081`.
- **-d** → Runs the container in **detached mode** (in the background).
- **springboot-docker-demo** → The Docker image to run.

**Output:**

- `22e33363536c29b5629465cf6004d921d1b14b0df5cc80eb2f5666231af72a19` → This is the **container ID**.

**2. View container logs**

```powershell
docker logs-f22e3
```

Explanation:

- **docker logs** → Shows logs from the container.
- **-f** → Follows the logs in real-time (like `tail -f`).
- **22e3** → Shortened container ID (you can use the first few characters of the full ID).

This is useful to check if your Spring Boot app started successfully.

**3. Stop a running container**

```powershell
docker stop 22e3
```

Explanation:

- **docker stop** → Gracefully stops a running container.
- **22e3** → Container ID of the running container.

After stopping, the app inside the container will no longer be running.

### Step 5: Steps to Push Docker Image to Docker Hub

Step 1: Login to Docker Hub

```bash
docker login
```

- Enter your Docker Hub username and password.

Step 2: Create a tag for your image

```bash
docker tag springboot-docker-demo chrisannlee/springboot-docker-demo:0.1.RELEASE
```

- Assigns a Docker Hub repository name and version to your local image.

Step 3: Push the Docker image to Docker Hub

```bash
docker push chrisannlee/springboot-docker-demo:0.1.RELEASE
```

- Uploads the image to Docker Hub.
- If public, anyone can pull it using:

```bash
docker pull chrisannlee/springboot-docker-demo:0.1.RELEASE
```
