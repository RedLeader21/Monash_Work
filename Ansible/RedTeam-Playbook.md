---
- name: My first playbook
  hosts: webservers
  become : true
  tasks:

  - name: Install Docker.io
    apt:
      update_cache: yes
      name: docker.io
      state: present

  - name: Install python3-pip
    apt:
      name: python3-pip
      state: present

  - name: Install Docker
    pip:
      name: docker
      state: present

  - name: download and launch a docker web container
    docker_container:
      name: dvwa
      image: cyberxsecurity/dvwa
      state: started
      restart_policy: always
      published_ports: 80:80

  - name: Enable docker service
    systemd:
      name: docker
      enabled: yes