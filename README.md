#AdGuard Home Docker Setup (Windows / PowerShell)

This guide helps you deploy **AdGuard Home** using Docker on a Windows machine to block ads and trackers on your home network.
Features covered - DNSSEC and DoH (more will be added with steps).

---

## Prerequisites

- Docker installed on your Windows PC  
- Basic knowledge of running **PowerShell commands**  
- Access to your router to update DNS settings  

---

## Setup Steps

### 1. Create a folder for AdGuard data

This folder stores configuration and runtime data, so it persists across container restarts.


```powershell
# Create folders for configuration and runtime data
New-Item -ItemType Directory -Path .\adguard\data\work -Force
New-Item -ItemType Directory -Path .\adguard\data\conf -Force
```
<img width="800" height="526" alt="image" src="https://github.com/user-attachments/assets/3270e65b-1207-48a6-9392-3ad156f7b204" />


###2. Create a docker-compose.yml file
Create a file named docker-compose.yml inside your project folder (.\adguard) and paste the following:
version: "3.8"
```
services:
  adguard:
    container_name: adguardhome
    image: adguard/adguardhome:latest
    restart: unless-stopped
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "3000:3000"
      - "80:80"
    volumes:
      - ./adguard/data/work:/opt/adguardhome/work
      - ./adguard/data/conf:/opt/adguardhome/conf
    environment:
      - TZ=Asia/Kolkata
      
```

3. Start the AdGuard Home container
Run this in PowerShell from the folder containing docker-compose.yml:
docker-compose up -d

This will start AdGuard Home in the background.

4. Access the AdGuard dashboard
Open a browser and visit:
http://<your-pc-ip>:3000

Replace <your-pc-ip> with your Windows machine’s IP address.

5. Configure your router’s DNS
Set your router’s primary DNS to your AdGuard server’s IP (<your-pc-ip>).
All network DNS queries will now pass through AdGuard Home for ad blocking.

6. Optional: Enable Advanced Features
In the AdGuard Home dashboard:
Enable DNSSEC for verified domain responses
Enable DoH / DoT for encrypted DNS queries

Replace the DNS servers as the following: 

<img width="900" height="534" alt="image" src="https://github.com/user-attachments/assets/a95d4f26-6fa9-415a-ae8d-4488b8f1666b" />

Export your settings regularly to back up configurations

7. Troubleshooting
Make sure ports 53, 80, and 3000 are not blocked by Windows Firewall
Check container logs for errors:
docker logs adguardhome

Restart container if needed:
docker-compose restart


References
AdGuard Home Official Repository
AdGuard Home Documentation

Happy ad-blocking!

---

