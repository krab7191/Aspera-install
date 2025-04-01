# Installing Console in a VM

## [Documentation](https://www.ibm.com/docs/en/aspera-console/3.4?topic=linux-installing-console)

## System for these instructions:
- Red Hat Enterprise Linux release 8.5 (Ootpa)
- 2 CPU, 8GB RAM, 100GB storage
- Ports open: 22, 80, 443, 8443, 9092, 33001

## File download links:
- General Aspera download page [here](https://www.ibm.com/products/aspera/downloads)
- [Console](https://ak-delivery04-mul.dhe.ibm.com/sdfdl/v2/sar/CM/OS/0cius/0/Xa.2/Xb.jusyLTSp44S04x7atuONoMA6TWF4XmS4B0pQhgF6jqe63gkUGYWbzwCJyTw/Xc.CM/OS/0cius/0/ibm-aspera-console-3.4.5.31-65f2112.x86_64.rpm/Xd./Xf.Lpr./Xg.13279244/Xi.habanero/XY.habanero/XZ.O4Z_EkfaOgR4xEz4UopFqU2Kcs51hNz6/ibm-aspera-console-3.4.5.31-65f2112.x86_64.rpm)
- [Common package](https://ak-delivery04-mul.dhe.ibm.com/sdfdl/v2/sar/CM/OS/0cius/0/Xa.2/Xb.jusyLTSp44S04x7atuNug4ilMUrbUiZcwDNaldBUcZc4NiV0l7je01IS4GA/Xc.CM/OS/0cius/0/ibm-aspera-common-1.6.2.40-db1a151.x86_64.rpm/Xd./Xf.Lpr./Xg.13279249/Xi.habanero/XY.habanero/XZ.7orlS4O9mQt7DGlYttmvXo0K3FHK_MDK/ibm-aspera-common-1.6.2.40-db1a151.x86_64.rpm)

## Instructions:

### Install Console
1. SSH into newly provisioned machine: `ssh -p 2223 -i pem_console.pem user@ip`
2. Go to /opt/software: `cd /opt/software`
3. Install Perl: `yum install -y perl`
4. Download console file: `curl --output console.rpm https://ak-delivery04-mul.dhe.ibm.com/sdfdl/v2/sar/CM/OS/0cius/0/Xa.2/Xb.jusyLTSp44S04x7atuONoMA6TWF4XmS4B0pQhgF6jqe63gkUGYWbzwCJyTw/Xc.CM/OS/0cius/0/ibm-aspera-console-3.4.5.31-65f2112.x86_64.rpm/Xd./Xf.Lpr./Xg.13279244/Xi.habanero/XY.habanero/XZ.O4Z_EkfaOgR4xEz4UopFqU2Kcs51hNz6/ibm-aspera-console-3.4.5.31-65f2112.x86_64.rpm`
5. Download common file: `curl --output common.rpm https://ak-delivery04-mul.dhe.ibm.com/sdfdl/v2/sar/CM/OS/0cius/0/Xa.2/Xb.jusyLTSp44S04x7atuNug4ilMUrbUiZcwDNaldBUcZc4NiV0l7je01IS4GA/Xc.CM/OS/0cius/0/ibm-aspera-common-1.6.2.40-db1a151.x86_64.rpm/Xd./Xf.Lpr./Xg.13279249/Xi.habanero/XY.habanero/XZ.7orlS4O9mQt7DGlYttmvXo0K3FHK_MDK/ibm-aspera-common-1.6.2.40-db1a151.x86_64.rpm`
6. Install: `yum localinstall -y console.rpm common.rpm`
7. Setup: `asctl console:setup`
8. Open console at \<ip>:\<port>
9. Add the license 
10. Change your password

### Add HSTS as a node in Console
1. Nodes -> New managed node
2. Add IP of HSTS machine, change port to 33001, click "Create default Console groups" -> Create.
3. Either do SSH or under Node API add creds of the `consoletransferuser` 

### Set up email config for console.
1. Notifications -> Email server
2. Enter SMTP details
