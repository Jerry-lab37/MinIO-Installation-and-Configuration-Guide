# MinIO Installation Guide

This document provides step-by-step instructions to install and configure MinIO on a Linux system.

## **Step 1: Update the System**

Update your package index and upgrade installed packages:
sudo apt update && sudo apt upgrade -y

Step 2: Download MinIO
Download the latest MinIO binary for the server:
wget https://dl.min.io/server/minio/release/linux-amd64/minio

Step 3: Make the Binary Executable
Make the MinIO binary executable:
chmod +x minio

Step 4: Move MinIO to a System-Wide Location
Move the binary to /usr/local/bin for system-wide access:
sudo mv minio /usr/local/bin/

Step 5: Create a User for MinIO
Create a dedicated user for MinIO:
sudo useradd -r minio-user -s /sbin/nologin

Step 6: Create MinIO Data and Configuration Directories
Set up directories for MinIO data and configuration:
sudo mkdir -p /mnt/data /etc/minio
sudo chown -R minio-user:minio-user /mnt/data /etc/minio

Step 7: Create a Systemd Service File
Create a systemd service file to manage MinIO:
sudo nano /etc/systemd/system/minio.service

Add the following content:

[Unit]
Description=MinIO
Documentation=https://min.io/docs/
Wants=network-online.target
After=network-online.target

[Service]
User=minio-user
Group=minio-user
EnvironmentFile=/etc/default/minio
ExecStart=/usr/local/bin/minio server /mnt/data
Restart=always
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target

Step 8: Configure MinIO
Create an environment file to configure MinIO:
sudo nano /etc/default/minio

Add the following content, replacing ACCESS_KEY and SECRET_KEY with your desired credentials (make sure to keep these secure and private):

MINIO_ROOT_USER=your-access-key
MINIO_ROOT_PASSWORD=your-secret-key
