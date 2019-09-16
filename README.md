# Dokumentation der Proxmox Server der UnixAG Zweibrücken

## Einrichtung des Basissystems

Diese Dokumentation geht von einer akutellen Proxmox VE Installation mit Minimalkonfiguration aus.

### Notwendige Software installieren und das System härten

Folgendes Skript als `root` ausführen:

```bash
apt update
apt install curl wget htop iftop iotop vim sudo fail2ban cron-apt -y
# cron-apt konfigurieren
echo "update -o quiet=2" > /etc/cron-apt/action.d/0-update
printf "autoclean -y\ndist-upgrade -y -o APT::Get::Show-Upgraded=true" > /etc/cron-apt/action.d/3-download
# fail2ban installieren
curl https://raw.githubusercontent.com/unixag-zw/proxmox-docs/master/assets/scripts/jail.local -o /etc/fail2ban/jail.local
curl https://raw.githubusercontent.com/unixag-zw/proxmox-docs/master/assets/scripts/proxmox.conf -o /etc/fail2ban/filters.d/proxmox.conf
# root-Login per SSH verbieten
sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/shh/sshd_config
service ssh restart
```
