## Ansible-openldap

#### Openldap 
Este rol installa Openldap y crea la base, dominio, y usuarios Admin y Developer en Ldap en los hosts especificados en inventory. Tambien carga los certificados de seguridad. 

* Playbook instalar openldap I.openldap.yml:
```
- hosts: ldap
  roles:
    - { role: openldap, openldap_server_domain_name: onuris-ldap.com, openldap_server_rootpw: changeit}
```
Las variables deben ser especificadas en el el playbook, si no se ejecutará con los valores default. 

* Ejecutar playbook:
```
ansible-playbook I.openldap.yml -i inventory
```
* Playbook desinstalar openldap R.openldap.yml
```
- hosts: ldap
  roles:
    - { role: remove-openldap }
```
* Ejecutar playbook: 
```
ansible-playbook R.openldap.yml -i inventory
```

#### Openldap HA

Este rol, hace que los hosts donde se ha instalado previamente Openldap se configuren en modo replica Master - Master. A partir de ahora cada cambio reflejado en uno de ellos se replicará en el otro y viceversa. La conexion entre ellos es a través de TLS. 

* Playbook I.HA-openldap.yml: 
```
- hosts: ldap
  roles:
    - { role: openldap-HA, openldap_server_domain_name: onuris-ldap.com, openldap_server_rootpw: changeit}
```
Deben especificarse el dominio y el root pasword del Openldap instalado previamente en los hosts. Este Rol, necesita de unas variables especificadas en el inventario:
```
[ldap]
Host1 ldap_provider=<host2> repl_id=1
Host2 ldap_provider=<host1> repl_id=2
```

* Ejecutar playbook: 
```
ansible-playbook I.HA-openldap.yml -i inventory

```

#### Phpldapadmin
Este rol instala la interfaz gráfica para poder gestionar Openldap. Una vez instalada, la interfaz gráfica estará disponible en : https://hostname:8080/phpldapadmin

* Playbook I.phpdaladmin.yml:
```
- hosts: ldap
  roles:
    - { role: phpldapadmin}
```
* Ejecutar playbook: 
```
ansible-playbook I.phpldapadmin -i inventory
```

* Playbook desinstalar openldap R.openldap.yml
```
- hosts: ldap
  roles:
    - { role: remove-phpldapadmin }
```
* Ejecutar playbook: 
```
ansible-playbook R.phpldapadmin -i inventory
```
