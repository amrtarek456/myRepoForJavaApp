---
- hosts: webservers 
  become: True
  tasks:
    - name: Install packages
      apt:
        name: "apache2"
        state: "present"
    - name: Start Apache server
      service:
        name: apache2
        state: started
        enabled: True
    - name: Deploy static website
      copy:
        src: index.html
        dest: /var/www/html/
    - name: Copy jar to backup
      copy:
        src: /var/lib/jenkins/workspace/nexus/MyWebApp/target/MyWebApp.jar
        dest: /var/lib/jenkins/workspace/nexus/MyWebApp/backup/backup.jar
...
