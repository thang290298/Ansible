<h1 align="center">Hướng dẫn cài đặt triển khài Ansible trên Ubuntu20.04</h1>

# I. Chuẩn bị
## 1. Mô hình triển khai

<h3 align="center"><img src="../03-Images/5.png"></h3>

Trong đó:
- 1 Node Server sử dụng Ubuntu 20.04
- 2 Node client
  - mỗi node có 3 disk bao gồm : 1 OS 25GB và 2 DATA 30DB

  ## 2. IP Planning

| Hostname | hardware | Interface | Disk OS | Disk 1 | Disk 2 | Disk 3 |
|--------------|-------|------|------|------|------|------|
| Node1-ctl | 3 CPU - 3GB RAM| ens3: 172.16.7.1 |25GB | 30GB | 30GB | 30GB |
| Node2 | 3 CPU - 3GB RAM| ens3: 172.16.7.2 |25GB | 30GB | 30GB | 30GB |
| Node3 |  3 CPU - 3GB RAM| ens3: 172.16.7.3|25GB | 30GB | 30GB | 30GB |