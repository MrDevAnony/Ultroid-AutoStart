# 🚀 Ultroid Auto Starter with systemd

This repository explains how to automatically start the [Ultroid](https://github.com/TeamUltroid/Ultroid) Telegram bot using `systemd` on Linux. With this setup, your bot will launch on every server reboot inside a detached `screen` session.

---

## ⚙️ Step-by-Step Setup

### Simple way to Run Ultroid:

```bash
git clone https://github.com/TeamUltroid/Ultroid
cd Ultroid
virtualenv -p /usr/bin/python3 venv
. ./venv/bin/activate
pip3 install --no-cache-dir  -r requirements.txt
pip3 install --no-cache-dir -r re*/st*/op*.txt
screen -S Ultroid
nano .env
bash startup
```

### 1. Create the startup script

Create a file named `start-ultroid.sh` in the root directory:

```bash
nano /root/start-ultroid.sh
```

Paste the following:

```bash
#!/bin/bash
cd /root/Ultroid || exit
screen -dmS Ultroid bash -c "
  source ./venv/bin/activate
  bash startup
"
```

Then make it executable:

```bash
chmod +x /root/start-ultroid.sh
```

### 2. Create the systemd service

Create the systemd service file:

```bash
nano /etc/systemd/system/ultroid.service
```

Paste the following content:

```bash
[Unit]
Description=Ultroid Bot Auto Starter
After=network.target

[Service]
Type=oneshot
User=root
ExecStart=/root/start-ultroid.sh
RemainAfterExit=true

[Install]
WantedBy=multi-user.target
```

### 3. Enable and start the service

Run the following commands to activate the service:

```bash
systemctl daemon-reexec
systemctl daemon-reload
systemctl enable ultroid.service
systemctl start ultroid.service
```

Done.

## 💡 Tips

To reattach to the `screen` session later:

```
screen -r Ultroid
```

🔌 Disable auto-start on boot:

```bash
systemctl disable ultroid.service
```

This will prevent the service from starting on the next system boot.

⛔ Stop the service immediately:
```bash
systemctl stop ultroid.service
```

This will stop the currently running instance (if active).

🧹 (Optional) Remove the service completely:

If you want to fully remove the service file:

```bash
rm /etc/systemd/system/ultroid.service
systemctl daemon-reload
```

⚠️ Make sure to disable it first before deleting the file.
