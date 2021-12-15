<h1 align="center">Ansible Role: Setup Nxlog on Windows Server 2016</h1>

## I. Thành Phần
- Nxlog
- Role
- Playbook

## II. Nội dung
### 1. Nxlog
- Một trình thu thập và chuyển tiếp nhật ký chung.
- Nxlog tương tự như syslog-ng hoặc rsyslog nhưng nó không chỉ giới hạn ở unix và syslog. Nó hỗ trợ các nền tảng, nguồn nhật ký và định dạng khác nhau để nxlog có thể là một lựa chọn lý tưởng để thực hiện một hệ thống ghi nhật ký tập trung
- Thu nhận nhật ký từ mạng từ xa qua UDP, TCP hoặc TLS / SSL trên tất cả các nền tảng được hỗ trợ
- Nó có khả năng lọc tin nhắn mạnh mẽ, viết lại nhật ký và khả năng chuyển đổi. Sử dụng một kiến ​​trúc nhẹ, mô-đun và đa luồng có thể mở rộng, nxlog có thể xử lý hàng trăm ngàn sự kiện mỗi giây.

<h3 align="center"><img src="../../../../03-Images/25.png"></h3>
<h3 align="center">Mô hình triển khai</h3>

### 2. Role
Thành Phần : 
- `default/main.yml` chứa thông tin về dường dẫn file thư mục cài đăt và cấu hình
```sh
---
INSTALLDIR: C:\Nxlog\
file_config: conf\
Folder_Agent: C:\AgentNxlog\
Agent_Nxlog: nxlog-trial-5.4.7313_windows_x64.msi
```

- `tasks/main.yml` thực thi các lệnh cài đặt và kiểm tra dịch vụ
```sh
- name: Installed Nxlog
  win_shell: msiexec /i {{ Folder_Agent }}\{{ Agent_Nxlog }} /qb INSTALLDIR={{ INSTALLDIR }}
```
  - check status nxlog
```sh
- name: Check Service status
  win_shell: Get-Service nxlog
  register: status

- debug: msg={{ status.stdout_lines }}
```

- `handler/main.yml`
```sh
---
- name: Restart-nxlog
  ansible.windows.win_service:
    name: nxlog
    state: restarted
```

### 3. Playbook
```sh
---
- name: Setup Nxlog on Windows Server
  hosts: Winserver

  roles:
      - Nxlog-Windows-Server
```
thực hiện chạy Setup và kết quả nhận được

<h3 align="center"><img src="../../../../03-Images/27.png"></h3>


<h3 align="center"><img src="../../../../03-Images/28.png"></h3>
