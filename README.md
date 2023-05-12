# Ansible

<h1>Actualiza el sistema:</h1>
<pre>sudo apt update</pre>
<pre>sudo apt upgrade -y</pre>
<h1>Instala Ansible:</h1>
<pre>sudo apt install software-properties-common</pre>
<pre>sudo apt-add-repository ppa:ansible/ansible</pre>
<pre>sudo apt install ansible -y </pre>

<h1>Siguiente paso:</h1>
<pre>cd /etc/ansible/</pre>
<pre>nano playbook.yml</pre>
<pre>
---

  hosts: webserver
  become: true
  tasks:
  - name: install Apache web server
    apt: 
      name=apache2
      state: present
    tags: apache
  - name: Instalar PHP
    apt:
      name:
      -php
      state: present

  hosts: database
  become: true
  tasks:
  - name: Instalar MySQL
    apt:
      name: mysql-server
      state: present

  - name: Copiar archivo de configuración
    copy:
      src: files/my.cnf
      dest: /etc/mysql/my.cnf
    notify:
        - Reiniciar MySQL

  - name: Crear base de datos
    mysql_db:
      name: test_db
      state: present
    notify:
        - Reiniciar MySQL

  - name: Crear usuario y otorgar privilegios
    mysql_user:
      name: user11
      password: password
      login_user: root
      login_password: password
      priv: "test_db.*:ALL"
      state: present
</pre>
<pre>nano hosts </pre>
<pre>
[webserver]
34.174.118.249
[database]
34.176.91.217
<pre># This is the default ansible 'hosts' file.
#
# It should live in /etc/ansible/hosts
#
#   - Comments begin with the '#' character
#   - Blank lines are ignored
#   - Groups of hosts are delimited by [header] elements
#   - You can enter hostnames or ip addresses
#   - A hostname/ip can be a member of multiple groups</pre>

<pre># Ex 1: Ungrouped hosts, specify before any group headers.

## green.example.com
## blue.example.com
## 192.168.100.1
## 192.168.100.10 </pre>

<pre># Ex 2: A collection of hosts belonging to the 'webservers' group

## [webservers]
## alpha.example.org
## beta.example.org
## 192.168.1.100
## 192.168.1.110 </pre>

<pre># If you have multiple hosts following a pattern you can specify
# them like this:</pre

<pre>## www[001:006].example.com</pre>
</pre>
<h1>Prueba de configuración de Ansible</h1>
<pre>ansible -m ping all</pre>
<h1>Para probar la conectividad de un host específico</h1>
<pre>ansible -m ping webserver</pre>
<pre>ansible -m ping database</pre>

<h1>Para ejecutar el playbook y hacer ping a ambas máquinas</h1>
<pre>ansible-playbook -i hosts playbook.yml</pre>
