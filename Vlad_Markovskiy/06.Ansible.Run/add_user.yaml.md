# Playbook of user adding
``` yaml
- hosts: debian
  become: True 
  tasks:
  - name: Print variable
    debug:
      msg: "User to create: {{ user_to_add }}"


  - name: Create user {{ user_to_add }}
    user:
      name: "{{ user_to_add }}"
      comment: Managed by ansible
      state: present


  - name: Sudo rights
    shell: |
      echo "{{ user_to_add }}   ALL=(ALL)       NOPASSWD: ALL" >> /etc/sudoers
    tags:
      - rights

  
  - name: Ssh dir creating
    file:
      path: /home/{{ user_to_add }}/.ssh
      state: directory
      owner: "{{ user_to_add }}"
      group: "{{ user_to_add }}"
      #mode: '0600'
    tags:
     - ssh::dir
 

  - name: Key kopying
    copy:
      src: /home/anjey/.ssh/id_rsa.pub
      dest: /home/{{ user_to_add }}/.ssh/authorized_keys
      owner: "{{ user_to_add }}"
      group: "{{ user_to_add }}"
      mode: '0600'
    tags:
      - key::cp

 # - name: upgrade
 #   shell: |
 #     apt-get upgrade
 #   register: shellout
 #   tags:
 #      - upgrade
 # - debug:
 #     msg: "{{ shellout.stdout }}"
#commited becouse of too long execution ( need to connect to fast ethernet and check (tested with mobile wifi spot) )


  - name: Check
    shell: |
      grep "{{ user_to_add }}" /etc/passwd
    register: out
  - debug:
      msg: "{{ out.stdout }}"




  - name: Removing user {{ user_to_add }}
    user:
      name: "{{ user_to_add }}"
      state: absent
    tags:
      - user::remove  
```