# Services and systemd — Commands Cheat Sheet

## Verify systemd
```bash
ps -p 1 -o comm=
```

## Service Control (`systemctl`)
```bash
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
sudo systemctl reload ssh
sudo systemctl enable --now ssh
sudo systemctl disable --now ssh
```

## Status & Boot State
```bash
systemctl status ssh
systemctl is-active ssh
systemctl is-enabled ssh
```

## Listing Services & Inspecting Unit Files
```bash
systemctl list-units --type=service
systemctl list-unit-files --type=service
systemctl cat ssh
```