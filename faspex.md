# Installing Faspex in a VM

## Requirements:
- Red Hat Enterprise Linux release 8.5 (Ootpa)
- 2 CPU, 8GB RAM, 100GB storage
- Ports open: 22, 80, 443, 8443, 9092, 33001

## [Documentation](https://www.ibm.com/docs/en/aspera-faspex/5.0?topic=upgrades-installing-faspex-first-time) 

## File download links:
- General Aspera download page [here](https://www.ibm.com/products/aspera/downloads)
- [Faspex](https://ak-delivery04-mul.dhe.ibm.com/sdfdl/v2/sar/CM/OS/0ct91/0/Xa.2/Xb.jusyLTSp44S04x7att5gbW-xyEdQTdjG-4dko_LTpMvoHv295dwJ4UdIHQo/Xc.CM/OS/0ct91/0/ibm-aspera-faspex-5.0.11.8306.x86_64.rpm/Xd./Xf.Lpr./Xg.13279199/Xi.habanero/XY.habanero/XZ.cOX3a56yAWv2kraEZSGh4vLN5YKnjrFu/ibm-aspera-faspex-5.0.11.8306.x86_64.rpm)

## Instructions:

### Install Faspex 
1. SSH into newly provisioned machine: `ssh -p 2223 -i pem_faspex.pem user@ip`
2. Install Docker:
   - `yum install -y yum-utils`
   - `yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo`
   - `yum install -y docker-ce docker-ce-cli`
   - `systemctl enable --now docker`
3. Install Faspex:
   - Go to /opt/software/faspex: `cd /opt/software/faspex/`
   - Get the Faspex file: `curl --output faspex.rpm https://ak-delivery04-mul.dhe.ibm.com/sdfdl/v2/sar/CM/OS/0ct91/0/Xa.2/Xb.jusyLTSp44S04x7att5gbW-xyEdQTdjG-4dko_LTpMvoHv295dwJ4UdIHQo/Xc.CM/OS/0ct91/0/ibm-aspera-faspex-5.0.11.8306.x86_64.rpm/Xd./Xf.Lpr./Xg.13279199/Xi.habanero/XY.habanero/XZ.cOX3a56yAWv2kraEZSGh4vLN5YKnjrFu/ibm-aspera-faspex-5.0.11.8306.x86_64.rpm`
   - `yum localinstall -y faspex.rpm`
4. Config is in `/opt/aspera/faspex/conf/docker/` if changes are needed.
5. Set up Faspex: `faspexctl setup`
6. After setup is complete, you can run `faspexctl status` to make sure all containers are running

### Configuration:
1. Open the Faspex web page at \<ip>:\<port>
2. Log in with the admin user created during the installation process
3. Add HSTS node and set default storage.

### Set up email config for Faspex:
1. Admin -> Configuration -> Email configuration
2. Enter SMTP details
