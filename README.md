cat_printer
=========

Deploy [Cat-Printer](https://github.com/NaitLee/Cat-Printer) as a [Podman Quadlet](https://github.com/containers/quadlet) container.  

Cat Printer is a web app daemon that allows communitcation with thermal printers
via bluetooth. The container must have access to the bluetooth device, which by
default will be `/dev/usb/hiddev0`. It's possible to adjust the device by
changing the variable `cat_printer_quadlet_devices`.  To figure out what is
your bluetooth device use the command `lsusb |grep -i bluetooth|awk -F':'
'{print $1}'|awk '{print "/dev/bus/usb/" $2 "/" $4}'`.  
This quadlet, by default will run as rootfull.  

Requirements
------------

* `podman` - version `4.7.2+`  

Role Variables
--------------

* `cat_printer_quadlet`: (`dict`) - Define the Container image and tag  
* `cat_printer_environment`: (`dict`) - Define the environment variables  
* `cat_printer_volumes`: (`list`) - A list of paths to be mounted  
* `cat_printer_quadlet_ports`: (`list`) - A list of ports to be exposed   
* `cat_printer_quadlet_devices`: (`list`) - A list of devices to bind in the
  container. (default: `/dev/usb/hiddev0` will be mounted)
* `cat_printer_autodetect_bluetooth`: (`bool`) - If enabled, tries to autodetect the
  bluetooth device  
* `cat_printer_dependecies`: (`list`) - List of packages to be installed as
  dependencies  

Dependencies
------------

None

Example Playbook
----------------

* Deploy using default settings:  

```yaml
---
- name: Deploy Cat Printer Quadlet
  hosts: all
  gather_facts: false
  cat_printer_quadlet_devices:
    - name: usb
      path: /dev/usb/hiddev1
      enable: true
  roles:
    - role: cat_printer
```

* Autodetect bluetooth:

```yaml
---
- name: Deploy Cat Printer Quadlet
  hosts: all
  gather_facts: false
  cat_printer_autodetect_bluetooth: true
  roles:
    - role: cat_printer
```


Developing and Testing
----------------------

This role was developed using [ansible
molecule](https://ansible.readthedocs.io/projects/molecule/).
The use of molecule is optional but recommended.  
  
* Testing:  
Unit tests for checking code regression are available in the [`tests` directory](tests/).
use the `verify` or `test` commands, e.g:  

```bash
molecule test
```

while developing use `verify` instead:  

```bash
molecule create
molecule verify
```

License
-------

[GPL-2.0-or-later](https://spdx.org/licenses/GPL-2.0-or-later.html)

Author Information
------------------

@mrbrandao - Igor Brandão
