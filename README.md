# Docker


**(What & Why)**

Development Operations
DevOps is a way of working that combines software development and IT operations, with the goal of delivering software faster, more reliably, and continuously through automation.
Automation is making everything work with the least amount of human interaction for these types of processes: testing, deployment, monitoring, and code integration.  
The key ideas here are to break down the wall between the developers and the operations team by using tools such as Docker, Kubernetes, Jenkins, and Git to make sure everything is automated and repeatable. The main idea is to focus on CI/CD (Continuous Integration, Continuous Delivery)

Now, let's talk about one of the tools that is used to do this job. Docker Compose is a tool that simplifies the process of managing complex applications that consist of multiple services, each running in its own Docker container. YAML is the file type that is used by Docker Compose to define the services of the application. It will hold the details of the Docker images to be used, port mappings, volumes for data persistence, network configurations, and dependencies between services.


**(Who is Who)**
The system we created is a simulation of a real-world trading system, where multiple services work together as one. We get to learn how modern data platforms are  structured, where they use streaming and multiple databases to handle different types of data. What Docker Compose does is make this setup available on any device with one click, ensuring consistency.
When we run the setup, all services start together (Zookeeper, Kafka, three databases, Anaconda). Docker will read the YAML file and create containers to run programs, networks to allow communication between them, and volumes to store data. 

![Docker YAML](https://github.com/user-attachments/assets/174c4cd2-309e-4b09-9ad3-a2f0f74396f6)

Relationships:

Zookeeper → Kafka: Kafka needs Zookeeper to organize brokers.

Kafka → Databases: Kafka streams incoming data into Cassandra, PostgreSQL, and InfluxDB.

Anaconda ↔ Kafka & Databases: Anaconda can send (produce) data into Kafka or read (consume) data from Kafka, and also query databases directly.

Connections:
These services connect via the trading-net, and they can call each other by their service names. As a host, I will be able to access these services through their ports:

Postgres → 5432

Cassandra → 9042

InfluxDB → 8086

Kafka → 29092

Anaconda → 8888

Lastly, we have the volumes where we store the data even if the service was deleted.



Installation step-by-step
-

## Step 1: Install Docker Desktop
- Download Docker Desktop from [Docker website](https://www.docker.com/products/docker-desktop/).
- Install it and make sure it is running.
<img width="1920" height="1200" alt="Screenshot (81)" src="https://github.com/user-attachments/assets/4d93b385-da05-4fc1-ab82-375d77c63b82" />

## Step 2: Create a Project Folder
Open a terminal and run:
mkdir trading-tools-docker
cd trading-tools-docker
<img width="1920" height="1200" alt="Screenshot (85)" src="https://github.com/user-attachments/assets/0a90b835-ece2-44e0-aa7e-895412488a2c" />

## Step 3: Create the docker-compose.yml File
Inside the folder, create a file named docker-compose.yml and paste the configuration.
<img width="1920" height="1200" alt="Screenshot (86)" src="https://github.com/user-attachments/assets/5b6f90db-2e40-431c-9b08-6990521cafb1" />

## Step 4: Start the Services
docker compose pull → downloads all the images (Kafka, Cassandra, etc.)
<img width="1920" height="1200" alt="Screenshot (88)" src="https://github.com/user-attachments/assets/180478b1-c5cc-480d-a44f-3030c67a9134" />

docker compose up -d → starts all the containers in detached mode.
<img width="1920" height="1200" alt="Screenshot (89)" src="https://github.com/user-attachments/assets/96cfdba9-84ce-475f-ae6e-853e250c4f5a" />

## Step 5: Verify Everything is Running
docker ps → This will list all running containers. You should see zookeeper, kafka, cassandra, postgres, influxdb, anaconda.

## Step 6: Access Each Service
PostgreSQL:
docker exec -it postgres psql -U admin -d tradingdb
<img width="1920" height="1200" alt="Screenshot (91)" src="https://github.com/user-attachments/assets/397c8f3a-02ed-4d9b-b1c4-7144c2cccc15" />

Cassandra:
docker exec -it cassandra cqlsh
<img width="1920" height="1200" alt="Screenshot (92)" src="https://github.com/user-attachments/assets/a5dd16de-67ca-45e6-86b2-61a91cc36c03" />

InfluxDB:
Open your browser at http://localhost:8086
Login: admin / admin12345
<img width="1920" height="1200" alt="Screenshot (93)" src="https://github.com/user-attachments/assets/482acd78-dcdd-4573-88c8-33e2cd704b8d" />

Kafka:
Reachable at localhost:29092. 

Anaconda:
docker exec -it anaconda bash
conda --version
<img width="1920" height="1200" alt="Screenshot (95)" src="https://github.com/user-attachments/assets/7c259e50-863e-4a11-b9f7-f97ce64a4c22" />

## Step 7: Networking
All services are connected through one virtual Docker network called trading-net. This allows them to reach each other by name (postgres, kafka, cassandra, etc.).

## Step 8: Persistence (Volumes)
Data is stored in volumes, so it doesn’t get lost when containers stop.
Check volumes:
docker volume ls
<img width="1920" height="1200" alt="Screenshot (96)" src="https://github.com/user-attachments/assets/af081051-7ea5-4947-9b69-1420d0db0dcc" />


