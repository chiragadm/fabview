# Fabview (fabview)
Fabview is python base CLI program that runs in Linux environment. It discovers Cisco MDS fabric and provide an ability to query FLOGI, switch port login and active zoning information.

# Installation:
Following are prerequisites for this CLI program.
* Linux based OS
* Python2.7 (python3 is not yet incorporated.)
  * Python Modules:
  * os
  * sys
  * re
  * pexpect
  * ConfigParser
  * StringIO
  * multiprocessing as mp
  * getpass
 
* # How to install this CLI program?
  Please run following command to install this CLI program. (You will need GIT CLI installed or you can copy the repository)
  * $ cd SOME_DIRECTORY (Directory that you want to install this program)
  * $ git clone https://URL

  # Setting up configuration file (fabview.cfg):
  Once the program is installed in directory as per above instruction, now it is time to configure the settings file.
  You will need to put the core switches information as per following. (Core switches are the switches that are connected to all other switches in your fabric)
  
  For example, I have two fabrics in my environment. Each fabric has one core switch. 
  * Fabric A core switch name is coreA.
  * Fabric B core switch name is coreB.
  
  With above example, my settings file (fabview.cfg) will look like below.
  Setting file is located in etc directory under your install directory.
  * $ cd SOME_DIRECTORY/etc
  * $ cat fabview.cfg
    ```
    [Fabric_Config]
    SWITCH: coreA,coreB
    SW_USER:
    SW_PASSWORD:
    ```
    
# How to use this CLI program?
Following is the CLI flags of this program.
```
 $ fabview -help
   usage:   fabview -update_cache || -alias < Alias Name/Prefix >

   -alias              : Alias Lookup
   -wwn                : WWN Alias Lookup
   -zone               : Zone Lookup
   -zoneset_info       : Active Zoneset Name Lookup for VSAN
   -switch_port        : Switch Port Flogi Lookup
   -update_cache       : Update Cache Files

   Example #1 :   fabview -alias coreA                       (Alias Lookup)
   Example #2 :   fabview -wwn ffffffffffffffff              (WWN Lookup)
   Example #3 :   fabview -zone ServerName                   (Zone Lookup)
   Example #4 :   fabview -zoneset_info coreB 1001           (Active Zoneset Name Lookup for VSAN)
   Example #5 :   fabview -switch_port coreA fc2/5           (Switch Port Flogi Lookup)
   Example #6 :   fabview -update_cache                      (Updates Cache Files Only)

```

You will first need to gather the cache before you can query information about the fabric. Please perform following command to gather cache.
```
 $ fabview -update_cache
   Enter Username for coreA, coreB: someuser
   Enter Password: somepassword

```

Following are the remaining examples on how to use this program.
```
 $ fabview -alias server1
host-edge1 port-channel20 1001 0x7f0001 20:00:00:00:00:00:00:3e SERVER1_HBA0
host-edge1 port-channel20 1001 0x7f0002 20:00:00:00:00:00:00:5e SERVER1_HBA6
host-edge1 port-channel20 1001 0x7f0003 20:00:00:00:00:00:00:7e SERVER1_HBA2
host-edge1 port-channel20 1001 0x7f0004 20:00:00:00:00:00:00:9e SERVER1_HBA4
host-edge2 port-channel21 1002 0x7f0011 20:00:00:00:00:00:00:4e SERVER1_HBA3
host-edge2 port-channel21 1002 0x7f0012 20:00:00:00:00:00:00:2e SERVER1_HBA5
host-edge2 port-channel21 1002 0x7f0013 20:00:00:00:00:00:00:8e SERVER1_HBA1
host-edge2 port-channel21 1002 0x7f0014 20:00:00:00:00:00:00:6e SERVER1_HBA7


$ fabview -wwn 200000000000003e
host-edge1 port-channel20 1001 0x7f0001 20:00:00:00:00:00:00:3e SERVER1_HBA0

or 

$ fabview -wwn 20:00:00:00:00:00:00:3e
host-edge1 port-channel20 1001 0x7f0001 20:00:00:00:00:00:00:3e SERVER1_HBA0


$ fabview -zone server1_hba0
coreA   zone name Fab-A_SERVER1_HBA0 vsan 1001
  * fcid 0x7f0001 [pwwn 20:00:00:00:00:00:00:3e] [SERVER1_HBA0]
  * fcid 0x7b000a [pwwn 50:00:00:00:00:14:9c:5a] [VMAX01-FA1]
  * fcid 0x7b000b [pwwn 50:00:00:00:00:14:ac:5a] [VMAX02-FA2]


$ fabview -zoneset_info coreA 1001
zoneset name Zoneset_FabA_VSAN1001 vsan 1001


$ fabview -switch_port storage-edge1 fc3/21
storage-edge1 fc3/21 1001 0x7b000a 50:00:00:00:00:14:9c:5a VMAX01-FA1
```
