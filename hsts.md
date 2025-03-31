# Installing the HSTS in a VM

## Requirements:
- Red Hat Enterprise Linux release 8.5 (Ootpa)
- 2 CPU, 8GB RAM, 100GB storage
- Ports open: 22, 80, 443, 8443, 9092, 33001

## [User management documentation](https://www.ibm.com/docs/en/ahts/4.4.x?topic=linux-set-up-users-groups-from-command-line) 

## File download links:
- General Aspera download page [here](https://www.ibm.com/products/aspera/downloads)
- [HSTS](https://ak-delivery04-mul.dhe.ibm.com/sdfdl/v2/sar/CM/OS/0clrf/1/Xa.2/Xb.jusyLTSp44S04x6s1JO0BshPWWJ_0hMdXKXUkcyywfCEiNdSUFXbEBkwLKY/Xc.CM/OS/0clrf/1/ibm-aspera-hsts-4.4.5.1646-linux-64-release.rpm/Xd./Xf.Lpr./Xg.13276532/Xi.habanero/XY.habanero/XZ.aTw600mjvH_m_7__TWdqYc5p9pUMHR2V/ibm-aspera-hsts-4.4.5.1646-linux-64-release.rpm)

## Instructions:

### Install HSTS 
1. SSH into newly provisioned machine: `ssh -p 2223 -i pem_hsts.pem user@ip`
2. Edit `/etc/ssh/sshd_config` by adding `Port 33001`
3. Restart the SSH daemon: `sudo systemctl restart sshd`. Subsequently, port 33001 can be used for SSH commands
4. Go to /opt/software: `cd /opt/software`
5. Get the HSTS file: `curl --output hsts.rpm https://ak-delivery04-mul.dhe.ibm.com/sdfdl/v2/sar/CM/OS/0clrf/1/Xa.2/Xb.jusyLTSp44S04x6s1JO0BshPWWJ_0hMdXKXUkcyywfCEiNdSUFXbEBkwLKY/Xc.CM/OS/0clrf/1/ibm-aspera-hsts-4.4.5.1646-linux-64-release.rpm/Xd./Xf.Lpr./Xg.13276532/Xi.habanero/XY.habanero/XZ.aTw600mjvH_m_7__TWdqYc5p9pUMHR2V/ibm-aspera-hsts-4.4.5.1646-linux-64-release.rpm`
6. Install HSTS: `yum localinstall -y hsts.rpm`
7. Install license: edit `/opt/aspera/etc/aspera-license` and add license contents
8. Set license permissions: `chmod 744 /opt/aspera/etc/aspera-license`
9. Check the installation: `ascp -A`
10. Set the Transfer Node IP address and SSH Port number: `asconfigurator -F "set_server_data;server_name,<ip>;ssh_port,33001"`
11. Enable activity logging: `asconfigurator -x "set_server_data;activity_logging,true"`
12. Configure transferred data location: `mkdir /data && chmod 777 /data`

### Prepare HSTS for Console Integration
1. Turn on async activity logging:
   - `asconfigurator -x "set_client_data;async_management_activity_logging,true"`
   - `asconfigurator -x "set_node_data;async_activity_logging,true"`
2. Create a console admin transfer user:
   - `useradd consoletransferadmin`
   - `asconfigurator -F \ "set_user_data;user_name,consoletransferadmin;absolute,/tmp/"`
   - `/opt/aspera/bin/asnodeadmin -a -u consoletransferadmin -p consolenodePW123$ -x consoletransferadmin --acl-set "admin,impersonation"`
3. Check users: `/opt/aspera/bin/asnodeadmin -l`

### Prepare HSTS for Faspex Integration
1. Make a faspex admin transfer user: `useradd faspextransferadmin`
2. Set up SSH auth:
   - `mkdir /home/faspextransferadmin/.ssh`
   - `chmod 700 /home/faspextransferadmin/.ssh/`
   - `cp /opt/aspera/var/aspera_tokenauth_id_rsa.pub /home/faspextransferadmin/.ssh/authorized_keys`
   - `chmod 600 /home/faspextransferadmin/.ssh/authorized_keys`
   - `chown -R faspextransferadmin:faspextransferadmin /home/faspextransferadmin/.ssh/`
3. Create a sub folder in /data where faspex files will be stored:
   - `mkdir /data/faspex_data`
   - `chown faspextransferadmin:faspextransferadmin /data/faspex_data`
4. Define token authorization for faspextransferadmin:
   - `asconfigurator -F "set_user_data;user_name,faspextransferadmin;authorization_transfer_in_value,token;authorization_transfer_out_value,token"`
5. Set token encryption string:
   - `asconfigurator -F "set_user_data;user_name,faspextransferadmin;token_encryption_key,14ba8b99-8f19-4151-ae0a-c8db9c6807c5"`
6. Define docroot storage for user:
   - `asconfigurator -F "set_user_data;user_name,faspextransferadmin;absolute,/data/faspex_data/"`
7. Check everything is set properly:
   - `ls -al /home/faspextransferadmin/.ssh`
   - `ls -lta /data/faspex_data`
   - `asconfigurator -F "get_user_data;user_name,faspextransferadmin" | grep in_value`
   - `asconfigurator -F "get_user_data;user_name,faspextransferadmin" | grep out_value`
   - `asconfigurator -F "get_user_data;user_name,faspextransferadmin" | grep token_encryption_key`
   - `asconfigurator -F "get_user_data;user_name,faspextransferadmin" | grep docroot`
8. Create a regular node user:
   - `/opt/aspera/bin/asnodeadmin -a -u faspexnodeuser -p faspexnodePW123$ -x faspextransferadmin`
   - List users: `/opt/aspera/bin/asnodeadmin -l`


