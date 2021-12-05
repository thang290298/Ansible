
chạy lệnh
```sh
ansible-playbook Unbuntu-docker.yml
```

- File: Unbuntu-docker.yml
  - Nội dung:
```sh
---
- name: Setup Docker on Ubuntu20.14
  hosts: hosts2
  become: yes

  roles:
      - Docker-ubuntu2004

```