This repo contains a complete and customizable Ansible project to configure ESF in a gateway. It can be used as a replacement of the `recovery_default_config.sh` script for recovering
the ESF factory configuration or to configure the gateway in dev-kit mode.

The playbook is designed to:

  1. unistall ESF from the gateway if present
  2. download the specified ESF package from the provided URL, if not already present in `iot-dev-kit/roles/esf_setup/files`
  3. install the ESF package
  4. copy the following files (if present in the `iot-dev-kit/roles/esf_setup/files`) in the correct path in the system:

  - interfaces
  - kuranet.conf
  - iptables
  - snapshot_0.xml

  5. reboot the gateway

# How to use the Ansible playbook
To properly configure ESF in a gateway, the whole folder has to be copied on the target device.
The following variables can be passed to the playbook:

  - **esf_name** the complete name of the ESF to be installed (i.e. esf-dynagate-20-30-el30)
  - **esf_installer_filename** the name of the ESF installer (i.e. esf-dynagate-20-30-el30-7.1.0-1.corei7_64.rpm )
  - **esf_download_url** the URL to download the ESF package
  - **esf_md5_checksum** the md5 of the installer in the form md5:XXXXXXX

The following example command can be used when the ESF installer has to be downloaded from a remote repo:
```
ansible-playbook iot-dev-kit/site.yml --extra-vars "esf_name=esf-dynagate-20-30-el30 esf_installer_filename=esf-dynagate-20-30-el30-7.1.0-1.corei7_64.rpm esf_download_url=http://172.16.0.100:8000/esf-dynagate-20-30-el30-7.1.0-1.corei7_64.rpm esf_md5_checksum=md5:713617f91a6493565a2c6a9106028563"
```

If the ESF installer is already present in the `iot-dev-kit/roles/esf_setup/files` folder, the following command can be used:
```
ansible-playbook iot-dev-kit/site.yml --extra-vars "esf_name=esf-dynagate-20-30-el30 esf_installer_filename=esf-dynagate-20-30-el30-7.1.0-1.corei7_64.rpm"
```

The user can customize the configuration files before applying the playbook. Moreover, the `iot-dev-kit/roles/linux_setup` folder contains a role that is run before the esf one 
to further customize the OS.
