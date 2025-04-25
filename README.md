# 📊 System Monitoring with Netdata

## 🚀 Project Goal

The goal of this project is to learn the **basics of system monitoring** by installing and configuring [Netdata](https://www.netdata.cloud/), a powerful real-time monitoring tool. By the end of this project, you'll know how to visualize system health, set alerts, and automate the setup.

---

## 🧰 Requirements

- ✅ Install Netdata on a Linux server
- ✅ Monitor CPU, memory, disk I/O
- ✅ Access dashboard via browser
- ✅ Customize one chart or section
- ✅ Set up an alert (e.g. CPU > 80%)
- ✅ Automate everything with scripts:
  - `setup.sh` — install Netdata
  - `test_dashboard.sh` — simulate system load
  - `cleanup.sh` — remove Netdata

---

## 📦 Manual Setup Steps (for learning)

### 1. 🖥️ Install Netdata

Run the official installation command:

```bash
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
```

> This installs Netdata agent and starts it as a systemd service.

### 2. 🌐 Access the Dashboard

Visit Netdata in your browser:

```bash
http://<your-server-ip>:19999
```

You’ll see a full real-time dashboard of system metrics.

---

### 3. 📈 Monitor Metrics

Out of the box, Netdata shows:
- CPU usage
- Memory & Swap
- Disk I/O
- Network traffic
- Load average
- Running processes

No extra config needed for basic system health.

---

### 4. 🛠 Customize Dashboard

Example: Hide a chart you don't care about.

Edit `/etc/netdata/netdata.conf` or use the dashboard UI to toggle chart visibility.

To hide charts via config:

```ini
[plugin:proc:/proc/stat]
	enabled = no
```

Restart Netdata:

```bash
sudo systemctl restart netdata
```

---

### 5. 🚨 Set Up Alerts

Example: Alert if CPU usage > 80%

Netdata alerts are configured via:

```bash
/etc/netdata/health.d/cpu.conf
```

Look for this section and modify if needed:

```yaml
alarm: cpu_usage
on: system.cpu
lookup: average -1m percentage of user
every: 10s
warn: $this > 80
```

Restart Netdata to apply changes.

---

## 🧪 Automation Scripts

### 🧰 `setup.sh` — Install Netdata

```bash
#!/bin/bash

echo "🔧 Installing Netdata..."
bash <(curl -Ss https://my-netdata.io/kickstart.sh)

echo "✅ Netdata installation completed."
echo "Visit http://$(hostname -I | awk '{print $1}'):19999 in your browser."
```

Make it executable:

```bash
chmod +x setup.sh
```

---

### 🔄 `test_dashboard.sh` — Simulate Load

```bash
#!/bin/bash

echo "📈 Simulating CPU load..."

# Start CPU stress test for 1 minute
sudo apt install -y stress
stress --cpu 2 --timeout 60 &

echo "🔥 Load running for 60 seconds... watch the dashboard!"
sleep 65

echo "✅ Load test complete."
```

---

### 🧹 `cleanup.sh` — Uninstall Netdata

```bash
#!/bin/bash

echo "🧼 Removing Netdata..."

sudo systemctl stop netdata
sudo systemctl disable netdata
sudo rm -rf /etc/netdata /var/lib/netdata /var/cache/netdata /usr/lib/netdata
sudo rm -rf /opt/netdata /etc/systemd/system/netdata*

echo "✅ Netdata removed successfully."
```

---

## 💡 Key Learnings

- Set up a real-time monitoring dashboard in minutes.
- Identified key system metrics and their importance.
- Understood how to trigger alerts based on thresholds.
- Learned to automate installation, testing, and cleanup with shell scripts — stepping into CI/CD mindset.

---

https://roadmap.sh/projects/simple-monitoring-dashboard
