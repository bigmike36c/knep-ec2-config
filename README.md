# KNEP EC2 Configuration

This repository contains the configuration files needed to run Kong Native Event Proxy (KNEP) on an EC2 instance with MSK connectivity.

## Overview

This setup provides:

- **KNEP Container**: Kong Native Event Proxy running on ports 8080, 8443, and exposing MSK through a virtual cluster on ports 19092-19095
- **Kafka Client**: kafkactl configured with SASL SCRAM-SHA-512 authentication
- **Docker Environment**: Complete containerized setup with Docker Compose

## Quick Setup for Existing EC2 Instance

If you already have an EC2 instance and want to prep manually, run these commands:

### 1. Update System and Install Docker

```bash
sudo yum update -y
sudo yum install -y docker
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker ec2-user
```

### 2. Install Docker Compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

### 3. Install kafkactl

```bash
sudo curl -L "https://github.com/deviceinsight/kafkactl/releases/download/v5.12.1/kafkactl_5.12.1_linux_amd64.tar.gz" -o /tmp/kafkactl.tar.gz
sudo tar -xzf /tmp/kafkactl.tar.gz -C /tmp
sudo mv /tmp/kafkactl /usr/local/bin/kafkactl
sudo chmod +x /usr/local/bin/kafkactl
sudo ln -s /usr/local/bin/kafkactl /usr/bin/kafkactl
sudo rm /tmp/kafkactl.tar.gz
```

### 4. Install Git and Clone Repository

```bash
sudo yum install -y git
git clone https://github.com/bigmike36c/knep-ec2-config.git /home/ec2-user/knep-config
sudo chown -R ec2-user:ec2-user /home/ec2-user/knep-config
cd /home/ec2-user/knep-config
```

### 5. Configure Environment

Create the `knep.env` file with your Kong Connect credentials (see `knep.env.example` for an example):

```bash
# Edit the environment file
vi knep.env
```

### 6. Start KNEP

```bash
docker-compose up -d
```

## Configuration Files

- **`docker-compose.yaml`**: KNEP container configuration
- **`knep.env`**: Kong Connect API credentials
- **`.kafkactl.yml`**: Kafka client configuration with SASL SCRAM-SHA-512

**Note**: Make sure to update the Kafka credentials in `.kafkactl.yml` with your actual username and password before connecting to your Kafka cluster.

To update Kafka credentials, edit `.kafkactl.yml`:

```bash
vi .kafkactl.yml
```
