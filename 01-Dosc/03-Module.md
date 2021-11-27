<h1 align="center">Tìm hiểu về Ansible-Module</h1>

# I. Các module thường được sử dụng trong ansible

## 1. Item
- Trường hợp ta cần cài đặt các gói httpd, mariadb-server, php-fpm...thay vì viết từng module cho các gói cài đặt ta sẽ thực hiện nhóm vào item để thực thi 1 lần chạy.

```sh
- hosts: hosts1
  become: true
  vars_files:
         - default.yml
  tasks:
    - name: Install latest version of Apache
      apt: name={{ item }} update_cache=yes tate=present
      with_items:
      - apache2
      - mysql-server
      - php7.2-mysql
      - php7.2
      - libapache2-mod-php7.2
      - python-mysqldb

    - name: Restart Apache2 and Mysql
      service:
        name: "{{item}}"
        state:  running
      with_items:
          - apache2
          - mysql
```

## 2. Handlers

- Handlers giúp chúng ta gọi lại hành động thực thi nhiều lần (notify) mà không cần phải viết lại.

```sh
- hosts: hosts1
  become: true
  vars_files:
         - default.yml
  tasks:
    - name: Install latest version of Apache
      apt: name={{ item }} update_cache=yes tate=latest
      with_items:
      - apache2
      - mysql-server
      - php7.2-mysql
      - php7.2
      - libapache2-mod-php7.2
      - python-mysqldb

    - name: Copy your index page
      template:
        src: "templates/index.html.j2"
        dest: "/var/www/{{ http_host }}/index.html"
      notify: restart service


  handlers:
    - name: restart service
      service:
        name: "{{ item }}"
        state:  restart
      with_items:
          - apache2
          - mysql
```

## 3. Variables và Template

- Đặt giá trị cho biến cố định
```sh
- hosts: hosts1
  become: true
  vars:
    - app_user: "ansible"
    - http_host: "Ansible.Playbook"
    - http_conf: "Playbook.conf"
    - http_port: "80"

  tasks:
    - name: Install latest version of Apache
      apt: name={{ item }} update_cache=yes tate=latest
      with_items:
      - apache2
      - mysql-server
      - php7.2-mysql
      - php7.2
      - libapache2-mod-php7.2
      - python-mysqldb

    - name: Copy your index page
      template:
        src: "templates/index.html.j2"
        dest: "/var/www/{{ http_host }}/index.html"
      notify: restart service


  handlers:
    - name: restart service
      service:
        name: "{{ item }}"
        state:  restart
      with_items:
          - apache2
          - mysql
```

## 4. FLIE
- Trong Ansible có nhiều module làm việc với tập tin, thư mục, links trên các node đích (node client) như copy, template, file,… 
- File module giúp quản lý tập tin và các thuộc tính của nó. Ngoài việc taọ, xóa, xác định vị trí của tệp tin file module cũng đặt các quyền và quyền sở hữu hay thiết lập symlinks cho tệp tin.

```sh
- hosts: hosts1
  become: true

  tasks:
    - name: Create document root for your domain
      file:
        path: "/var/www/{{ http_host }}"
        state: directory
        owner: "{{ app_user }}"
        group: "{{ app_group }}"
        mode: '0755'

    - name: Change file ownership, group and permissions
      file:
        path: /etc/demo.config
        owner: "{{ app_user }}"
        group: "{{ app_group }}"
        mode: '0644'

    - name: Create a symbolic link
      file:
        src: /etc/apache2/sites-available/{{ http_conf }}
        dest: /etc/apache2/sites-enabled/
        owner: root
        group: root
        state: link
```

## 5. Coppy
- Copy module là module thường được sử dụng khi chúng ta muốn sao chép một tệp tin từ Ansible server đến các node client

```sh
- name: copy file from local to remote with root as the owner (become required)
  copy:
    src: test_file
    dest: "/home/{{ ansible_user }}/test_file"
    owner: root
    group: root
    mode: 0644
  become: true
```
## 6. Service

- Đối với các node client là Unix/Linux. Service module là một module rất hữu ích giúp kiểm soát các service chạy trên các server này
- Sử dụng các tham số này và các giá trị bắt buộc, các bạn có thể quản lý các service với các chức năng như stop, start, reload, ... trên các node client.

```sh
- name: Start service httpd, if not running
  service:
    name: httpd
    state: started

- name: Stop service httpd, if running
  service:
    name: httpd
    state: stopped

- name: Restart service httpd, in all cases
  service:
    name: httpd
    state: restarted

- name: Reload service httpd, in all cases
  service:
    name: httpd
    state: reloaded

- name: Enable service httpd, and not touch the running state
  service:
    name: httpd
    enabled: yes

```

# II. Tài liệu Tham Khảo


- [1] https://github.com/phancong0897/Congphan/blob/master/Ansible/L%C3%BD%20Thuy%E1%BA%BFt/Module-ansible.md
