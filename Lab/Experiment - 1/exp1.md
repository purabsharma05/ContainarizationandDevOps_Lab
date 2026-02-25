# Experiment 1

## Title
Compare Virtual Machine and Container

---

## Objective
To study Virtual Machines and Containers, deploy services using both approaches, analyze system resource usage, and conclude which virtualization technique is more efficient.

---

## Tools Used
- Oracle VirtualBox  
- Vagrant  
- Ubuntu (Jammy 22.04)  
- Nginx  
- Docker  
- Windows Task Manager  

---

## Theory

Virtual Machine (VM) and Container are two different approaches to virtualization.

A Virtual Machine provides hardware-level virtualization and runs a complete operating system. Each VM includes its own OS, libraries, and dependencies. Due to this, Virtual Machines consume more CPU, memory, and storage resources. VM startup time is also higher.

Containers provide operating system-level virtualization. Containers share the host operating system kernel and only package the application with its dependencies. This makes containers lightweight, faster, and more resource-efficient.

In this experiment, a Virtual Machine is created using VirtualBox and Vagrant, and Nginx is installed inside it. Later, the same service is deployed using Docker containers. Resource usage is compared to determine which approach performs better.

---

## Procedure

### Part A: Virtual Machine Setup

#### Step 1: Initialize Ubuntu Virtual Machine

```bash
vagrant init ubuntu/jammy64
```
![Vagrant init](./images/image_1.png)

---

#### Step 2: Start the Virtual Machine
```bash
vagrant up
```
![Vagrant up](./images/image_2.png)

---

#### Step 3: Access Ubuntu VM
```bash
vagrant ssh
```
![Vagrant ssh](./images/image_3.png)

#### Step 4: Install Nginx inside VM
```bash
sudo apt update
sudo apt install nginx
sudo systemctl start nginx
```

#### Step 5: Verify Nginx in VM
```bash
curl localhost
```
![Nginx running](./images/image_4.png)

#### Step 6: Observe Resource Usage (VM)

CPU and memory usage were monitored while VM was running.
![VM resource usage](./images/image_5.png)



### Part B: Container Setup (Docker)
#### Step 7: Pull and Run Nginx Container
```bash
docker pull nginx
docker run -d -p 8080:80 --name nginx-container nginx
```
![Docker commands](./images/image_6.png)

#### Step 8: Verify Nginx in Container
```bash
curl localhost:8080
```
![Container verification](./images/image_7.png)

#### Step 9: Observe Docker Resource Usage

Resource usage of Docker containers was monitored.
![Docker resource usage](./images/image_8.png)


### Commands Used
```bash
vagrant init ubuntu/jammy64
vagrant up
vagrant ssh
sudo apt update
sudo apt install nginx
sudo systemctl start nginx
curl localhost

docker pull nginx
docker run -d -p 8080:80 --name nginx-container nginx
docker images
docker ps
```

### Observation

Through this experiment, we observed that:

- The Virtual Machine consumed significantly higher CPU and memory resources because it runs a complete operating system kernel
- The Docker container started faster and consumed fewer resources while running the same Nginx service
- Docker provides better resource efficiency for application deployment compared to traditional Virtual Machines
### Conclusion

Containers are more efficient than Virtual Machines. Containers are lightweight, start faster, and consume fewer system resources because they share the host operating system kernel. This experiment clearly demonstrates that container-based virtualization is better suited for modern application deployment compared to traditional Virtual Machines.