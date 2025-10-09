
# DevOps Final Project – Docker Compose System

## Project Overview

This project demonstrates the implementation of a multi-service environment using **Docker Compose**, representing a real-world data-driven trading system.
The purpose is to showcase how **DevOps principles** such as automation, containerization, and continuous integration can be applied to deploy complex, interconnected services with minimal manual configuration.


## Understanding DevOps and Docker

**DevOps** is a methodology that integrates software development and IT operations to improve collaboration, speed, and reliability.
Its goal is to automate repetitive tasks like testing, deployment, and monitoring ensuring **Continuous Integration (CI)** and **Continuous Delivery (CD)**.

**Docker** enables developers to package applications and their dependencies into portable containers that can run consistently across any environment.
**Docker Compose**, in particular, simplifies managing multi-container applications using a single configuration file (`docker-compose.yml`).

In this project, Docker Compose automates the creation of containers, networks, and volumes for multiple interconnected services.
This approach simulates how modern companies build scalable data pipelines and analytic platforms.


## System Architecture Overview

The system simulates a **trading-data platform** composed of several services working together, each running inside its own container.
Docker Compose ensures these services launch simultaneously, interact seamlessly, and maintain consistent configurations.

![Docker YAML](https://github.com/user-attachments/assets/174c4cd2-309e-4b09-9ad3-a2f0f74396f6)

### Components and Relationships

* **Zookeeper → Kafka**: Zookeeper manages Kafka brokers and coordinates their communication.
* **Kafka → Databases**: Kafka streams incoming data into Cassandra, PostgreSQL, and InfluxDB.
* **Anaconda ↔ Kafka & Databases**: Anaconda can produce and consume data through Kafka, while also querying databases directly.

### Network Connections

All services are connected through a custom Docker network (`trading-net`), which allows them to resolve each other by name.

| Service    | Port  | Purpose                    |
| ---------- | ----- | -------------------------- |
| PostgreSQL | 5432  | Relational database access |
| Cassandra  | 9042  | Distributed NoSQL database |
| InfluxDB   | 8086  | Time-series database       |
| Kafka      | 29092 | Message streaming service  |
| Anaconda   | 8888  | Data analytics environment |

Persistent **volumes** ensure that data is retained even when containers are stopped or recreated.


## Step-by-Step Installation Guide (Windows)

### Step 1: Install Docker Desktop

Download and install Docker Desktop from the [official website](https://www.docker.com/products/docker-desktop/).
Once installed, ensure Docker Desktop is running.

<img width="1920" height="1200" alt="Screenshot (81)" src="https://github.com/user-attachments/assets/4d93b385-da05-4fc1-ab82-375d77c63b82" />


### Step 2: Create a Project Folder

Open the terminal inside Docker Desktop and create a directory for the project:

```bash
mkdir trading-tools-docker
cd trading-tools-docker
```

<img width="1920" height="1200" alt="Screenshot (85)" src="https://github.com/user-attachments/assets/0a90b835-ece2-44e0-aa7e-895412488a2c" />


### Step 3: Create the `docker-compose.yml` File

Inside the folder, create a new file named `docker-compose.yml` and paste the system configuration into it.

<img width="1920" height="1200" alt="Screenshot (86)" src="https://github.com/user-attachments/assets/5b6f90db-2e40-431c-9b08-6990521cafb1" />


### Step 4: Start the Services

To pull all required images (Kafka, Cassandra, PostgreSQL, etc.):

```bash
docker compose pull
```

<img width="1920" height="1200" alt="Screenshot (88)" src="https://github.com/user-attachments/assets/180478b1-c5cc-480d-a44f-3030c67a9134" />

Then start all containers in detached mode:

```bash
docker compose up -d
```

<img width="1920" height="1200" alt="Screenshot (89)" src="https://github.com/user-attachments/assets/96cfdba9-84ce-475f-ae6e-853e250c4f5a" />


### Step 5: Verify Running Containers

To confirm that all containers are active:

```bash
docker ps
```

You should see `zookeeper`, `kafka`, `cassandra`, `postgres`, `influxdb`, and `anaconda` listed.

<img width="1920" height="1200" alt="Screenshot (90)" src="https://github.com/user-attachments/assets/b3005791-941a-4caa-81d1-cd47e4eb0e04" />


### Step 6: Access Each Service

**PostgreSQL**

```bash
docker exec -it postgres psql -U admin -d tradingdb
```

<img width="1920" height="1200" alt="Screenshot (91)" src="https://github.com/user-attachments/assets/397c8f3a-02ed-4d9b-b1c4-7144c2cccc15" />

**Cassandra**

```bash
docker exec -it cassandra cqlsh
```

<img width="1920" height="1200" alt="Screenshot (92)" src="https://github.com/user-attachments/assets/a5dd16de-67ca-45e6-86b2-61a91cc36c03" />

**InfluxDB**
Open your browser at [http://localhost:8086](http://localhost:8086)
Login credentials:

* Username: `admin`
* Password: `admin12345`

<img width="1920" height="1200" alt="Screenshot (93)" src="https://github.com/user-attachments/assets/482acd78-dcdd-4573-88c8-33e2cd704b8d" />

**Kafka**
Reachable at `localhost:29092`.

**Anaconda**

```bash
docker exec -it anaconda bash
conda --version
```

<img width="1920" height="1200" alt="Screenshot (95)" src="https://github.com/user-attachments/assets/7c259e50-863e-4a11-b9f7-f97ce64a4c22" />


## Networking and Data Persistence

All containers are connected through a single virtual Docker network named `trading-net`, enabling inter-container communication by service name.
This eliminates the need for IP-based access and ensures easy scaling.

### Persistent Volumes

Docker volumes retain data even after containers are removed.
You can list existing volumes with:

```bash
docker volume ls
```

<img width="1920" height="1200" alt="Screenshot (96)" src="https://github.com/user-attachments/assets/af081051-7ea5-4947-9b69-1420d0db0dcc" />


## Learning Outcomes

Through this project, the following key DevOps and containerization concepts were demonstrated and understood:

1. **DevOps Automation:** Implementing CI/CD principles by automating deployment using Docker Compose.
2. **Containerization:** Building isolated, reproducible environments for multiple services.
3. **Networking:** Understanding how containers communicate via custom Docker networks.
4. **Data Persistence:** Using volumes to maintain data consistency beyond container lifecycles.
5. **Service Orchestration:** Running interconnected tools (Kafka, PostgreSQL, Cassandra, InfluxDB, Anaconda) in one coordinated environment.
6. **Real-World Simulation:** Replicating the architecture of a streaming-based trading data platform.
7. **Troubleshooting Skills:** Learning to inspect, restart, and manage services using Docker CLI commands.


## Conclusion

This project highlights the power of Docker and Docker Compose in simplifying complex system setups and promoting DevOps best practices.
By automating multi-service deployment, maintaining consistency across environments, and ensuring data reliability through volumes, this system serves as both a practical demonstration and a learning milestone in containerized infrastructure management.

