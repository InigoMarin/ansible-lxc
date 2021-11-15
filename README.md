# Ejemplo de prueba de lxd con ansible

- Instalar ansible
```
sudo apt install ansible
```

- Actualizar plugins ansible de la comunidad
```
ansible-galaxy collection install community.general -f
```
- Crear maquinas lxd de ejemplo
```
lxc launch ubuntu:20.04 c1
lxc launch ubuntu:20.04 v1 --vm
```

- Crear ficher ansible.cfg
```

[defaults]
inventory = hosts.yaml
```

- Crear fichero hosts.yaml
```
all:
  vars:
    ansible_connection: lxd
    ansible_user: root
    ansible_become: no
  children:
    local:
      vars:
        ansible_lxd_remote: local
        ansible_lxd_proyect: demo
      hosts:
        c1:
        v1:

```
- Testear inventario de invisible con el comando

```
ansible-inventory --graph
```

- Crear fichero de tareas para instalar apache2
```
- name : Setup apache
  hosts: all
  tasks:
    - name : Install the apache package
      apt:
        name: apache2
        state: present

    - name : Make sure apache is running
      systemd:
        name: apache2.service
        state: started
```

- Ejecutar tare de ansible
```
ansible-playbook demo.yaml -l local
```
