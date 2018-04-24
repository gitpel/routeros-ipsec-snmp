# Easy way to handle IPsec Tunnel Status on RouterOS\Mikrotik using SNMP
## 1. How it works:
1. SNMP Asks for OID: `1.3.6.1.4.1.14988.1.1.18.1.1.2.X` (Where last «X» is an SNMP ID number of a script in RouterOS\Mikrotik `/system script`)
2. RouterOS\Mikrotik automatically runs the script and returns a result to SNMP with 0 or 1 if IPsec Phase 2 Established with the IP
## 2. How to use it:
1. Create a copy of the script (https://raw.githubusercontent.com/gitpel/routeros-ipsec-snmp/master/snmp-ipsec-ph2-probe) to your RouterOS/Mikrotik `system > scripts`
2. Name it for example as "probe-ipsec-x.x.x.x"
3. Enable SNMP on RouterOS\Mikrotik (we will use the version of SNMP v2c):
###### * we will need SNMP write access to execute scripts via SNMP
```
/snmp community set [ find default=yes ] addresses=0.0.0.0/0 write-access=yes
/snmp set contact="XXXXXXXXX" enabled=yes location="YYYYYYYYYY" trap-version=2
```
4. Download https://www.paessler.com/tools/snmptester for SNMP debugging also you can use *nix `snmpget` and `snmpwalk` (more info: https://wiki.mikrotik.com/wiki/Manual:SNMP#Run_Script)
5. Check OIDs of scripts that you have on your RouterOS\Mikrotik using SNMP walk and OID for scripts: `1.3.6.1.4.1.14988.1.1.8`
![alt text](https://raw.githubusercontent.com/gitpel/routeros-ipsec-snmp/master/snmp_tester_img_01.png "Paessler SNMP Tester")
###### As you can see in my example I have 2 scripts with OIDs: 1.3.6.1.4.1.14988.1.1.8.1.1.2.1 and 1.3.6.1.4.1.14988.1.1.8.1.1.2.2, I will use the first one for example
6. Change **8** to **18** in OID: 1.3.6.1.4.1.14988.1.1.**8**.1.1.2.1 to 1.3.6.1.4.1.14988.1.1.**18**.1.1.2.1 to execute the script via SNMP and get results
6. Check result of the script that you have on your RouterOS\Mikrotik using SNMP get
![alt text](https://raw.githubusercontent.com/gitpel/routeros-ipsec-snmp/master/snmp_tester_img_02.png "Paessler SNMP Tester")
7. Add the SNMP probe check to your SNMP Management System (In my case we will use PRTG)
8. Done
#### Now using SNMP you can ask and receive a result of the status of yours IPsec Tunnels
#### Using the same way you can get any other data from RouterOS\Mikrotik
