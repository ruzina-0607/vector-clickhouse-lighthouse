## Lighthouse
This role can install and configure Lighthouse на Centos 7

## Requirements
You neeed to install and configure git and NGINX (or another web server) before run this role.

## Переменные
| vars	| Description	| Value	| Location |
|-------|-------------|-------|----------|
|lighthouse_dir|	Where to store Lighthouse files	| "/home/{{ ansible_user_id }}/lighthouse"	| defaults/main.yml |
|lighthouse_url|	URL of Clickhouse repo	| "https://github.com/VKCOM/lighthouse.git"	| vars/main.yml |

Playbook
```bash
- name: Install lighthouse
  hosts: lighthouse
  handlers:
    - name: Nginx reload
      become: true
      ansible.builtin.service:
        name: nginx
        state: restarted
  pre_tasks:
    - name: Lighthouse | Install git
      become: true
      ansible.builtin.yum:
        name: git
        state: present
    - name: Lighhouse | Install nginx
      become: true
      ansible.builtin.yum:
        name: nginx
        state: present
    - name: Lighthouse | Apply nginx config
      become: true
      ansible.builtin.template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
        mode: 0644
  roles:
    - lighthouse
  post_tasks:
    - name: Lighthouse | Apply config
      become: true
      ansible.builtin.template:
        src: lighthouse_nginx.conf.j2
        dest: /etc/nginx/conf.d/lighthouse.conf
        mode: 0644
        notify: Nginx reload 
```
Meta
```bash
galaxy_info:
  author: Ruzina
  description: role for install and configure Lighthouse
  company: netology
  role_name: lighthouse_role
  namespace: name
```
