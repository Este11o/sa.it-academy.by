# Inventory file
```yaml
internal:
  hosts:
    host1:
      ansible_host: 192.168.55.2
    host2:
      ansible_host: 192.168.55.3


jump:
  hosts:
    bastion:
      ansible_host: 192.168.43.40


debian:
  hosts:
    deb_host:
      ansible_host: 192.168.55.3   
#expiremental:
#  hosts:  
 #   byrik:
  #    ansible_host: 192.168.43.10 ansible_user=pushkin
## Just expirement with my friend notebook
```