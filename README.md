# Dummy systemd Service

## Project URL
This project is an implementation of a dummy systemd service based on the DevOps roadmap task:  
https://roadmap.sh/projects/dummy-systemd-service

---

## General Information

This project demonstrates how to create and manage a custom `systemd` service.

### Key Concepts

- **systemd** — init system and service manager used in most modern Linux distributions.
- **unit** — a resource managed by systemd (e.g., service, mount point, device).
- **unit file** — a configuration file that defines how a unit behaves.
- **service unit (`.service`)** — a unit type used to manage background processes.

### Important Notes

- Custom unit files should be placed in: `/etc/systemd/system/`
This prevents them from being overwritten by package updates.

- systemd uses a **declarative configuration model**, meaning you describe *what should happen*, not *how step-by-step*.

---

## Repository Structure
.<br /> 
├── dummy.service # systemd unit file<br /> 
├── dummy.sh # script executed by the service<br /> 
└── README.md # project documentation <br />

---

## Script Overview

`dummy.sh` is a simple infinite loop that simulates a long-running background service:

```bash
#!/bin/bash

while true; do
    echo "Dummy service is running..." >> /var/log/dummy-service.log
    sleep 10
done
```

### Behavior
- Runs indefinitely
- Writes a log entry every 10 seconds
- Simulates a real background daemon

### Service File Overview
[Unit]<br /> 
Description — human-readable description<br /> 
After=multi-user.target — ensures the service starts after the system reaches multi-user mode<br /> 
[Service]<br /> 
ExecStart — path to the script<br /> 
Type=simple — service starts immediately<br /> 
Restart=always — automatically restarts if it stops<br /> 
StandardOutput=journal — logs go to systemd journal<br /> 
StandardError=journal<br /> 
SyslogIdentifier=dummy — label used in logs<br /> 
[Install]<br /> 
WantedBy=multi-user.target — enables service to start at boot<br /> 

## How to Run

1. Clone the repo 
2. Move the script to /usr/local/bin/ and make it executable with `chmod +x`
3. Move the service file to /etc/systemd/system
4. Reload systemd `sudo systemctl daemon-reload`
5. Enable the service `sudo systemctl enable dummy.service`
6. Start the service `sudo systemctl start dummy.service`
7. Check status `systemctl status dummy.service`
8. View logs `journalctl -u dummy.service -f`

### Output example

[ilgar@IlgArch 09_Dummy_Systemd_Service]$ cat /var/log/dummy-service.log<br />  
Dummy service is running...<br /> 
Dummy service is running...<br /> 
Dummy service is running...<br /> 
Dummy service is running...<br /> 
Dummy service is running...<br /> 
Dummy service is running...<br /> 
Dummy service is running...<br /> 
Dummy service is running...<br /> 
Dummy service is running...<br /> 
Dummy service is running...<br /> 
Dummy service is running...<br /> 
Dummy service is running...<br /> 
[ilgar@IlgArch 09_Dummy_Systemd_Service]$ journalctl --unit=dummy.service<br /> 
Apr 21 22:26:24 IlgArch systemd[1]: Started Dummy Script.<br /> 
[ilgar@IlgArch 09_Dummy_Systemd_Service]$<br />
