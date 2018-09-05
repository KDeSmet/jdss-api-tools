
 # jdss-api-tools


<b>Execute given JovianDSS command for automated setup and to control JovianDSS remotely.</b>


 EXAMPLES:

 1. <b>Create clone</b> of iSCSI volume zvol00 from Pool-0 and attach to iSCSI target.

    Every time it runs, it will delete the clone created last run and re-create new one.
    So, the target exports most recent data every run.
    The example is using default password and port.
    Tools automatically recognize the volume type. If given volume is iSCSI volume,
    the clone of the iSCSI volume will be attached to iSCSI target.
    If given volume is NAS dataset, the created clone will be exported via network share
    as shown in the next example.

        jdss-api-tools.exe clone --pool Pool-0 --volume zvol00 --node 192.168.0.220

 2. <b>Create clone</b> of NAS volume vol00 from Pool-0 and share via new created SMB share.

    Every time it runs, it will delete the clone created last run and re-create new one.
    So, the share exports most recent data every run. The share is invisible by default.
    The example is using default password and port and make the share visible with default share name.

        jdss-api-tools.exe clone --pool Pool-0 --volume vol00 --visible --node 192.168.0.220

    The examples are using default password and port and make the shares invisible.
     
        jdss-api-tools.exe clone --pool Pool-0 --volume vol00 --share_name vol00_backup --node 192.168.0.220
        jdss-api-tools.exe clone --pool Pool-0 --volume vol01 --share_name vol01_backup --node 192.168.0.220

 3. <b>Delete clone</b> of iSCSI volume zvol00 from Pool-0.

        jdss-api-tools.exe delete_clone --pool Pool-0 --volume zvol00 --node 192.168.0.220

 4. <b>Delete clone</b> of NAS volume vol00 from Pool-0.

        jdss-api-tools.exe delete_clone --pool Pool-0 --volume vol00 --node 192.168.0.220

 5. <b>Create clone</b> of existing snapshot on iSCSI volume zvol00 from Pool-0 and attach to iSCSI target.

    The example is using password 12345 and default port.

        jdss-api-tools.exe clone_existing_snapshot --pool Pool-0 --volume zvol00 --snapshot autosnap_2018-06-07-080000 --node 192.168.0.220 --pswd 12345

 6. <b>Create clone</b> of existing snapshot on NAS volume vol00 from Pool-0 and share via new created SMB share.

    The example is using password 12345 and default port.

        jdss-api-tools.exe clone_existing_snapshot --pool Pool-0 --volume vol00 --snapshot autosnap_2018-06-07-080000 --node 192.168.0.220 --pswd 12345

 7. <b>Delete clone</b> of existing snapshot on iSCSI volume zvol00 from Pool-0.

    The example is using password 12345 and default port.

        jdss-api-tools.exe delete_clone_existing_snapshot --pool Pool-0 --volume zvol00 --snapshot autosnap_2018-06-07-080000 --node 192.168.0.220 --pswd 12345

 8. <b>Delete clone</b> of existing snapshot on NAS volume vol00 from Pool-0.

    The example is using password 12345 and default port.

        jdss-api-tools.exe delete_clone_existing_snapshot --pool Pool-0 --volume vol00 --snapshot autosnap_2018-06-07-080000 --node 192.168.0.220 --pswd 12345

 9. <b>Create pool</b> on single node or cluster with single JBOD:

    Pool-0 with 2 * raidz1(3 disks) total 6 disks 

        jdss-api-tools.exe create_pool --pool Pool-0 --vdevs 2 --vdev raidz1 --vdev_disks 3 --node 192.168.0.220

10. <b>Create pool</b> on Metro Cluster with single JBOD with 4-way mirrors:

    Pool-0 with 2 * mirrors(4 disks) total 8 disks 

        jdss-api-tools.exe create_pool --pool Pool-0 --vdevs 2 --vdev mirror --vdev_disks 4 --node 192.168.0.220

11. <b>Create pool</b> with raidz2(4 disks each) over 4 JBODs with 60 HDD each.

    Every raidz2 vdev consists of disks from all 4 JBODs. An interactive menu will be started.
    In order to read disks, POWER-ON single JBOD only. Read disks selecting "0" for the first JBOD.
    Next, POWER-OFF the first JBOD and POWER-ON the second one. Read disks of the second JBOD selecting "1".
    Repeat the procedure until all JBODs disk are read. Finally, create the pool selecting "c" from the menu.

        jdss-api-tools.exe create_pool --pool Pool-0 --jbods 4 --vdevs 60 --vdev raidz2 --vdev_disks 4 --node 192.168.0.220

12. <b>Shutdown</b> three JovianDSS servers using default port but non default password.

        jdss-api-tools.exe --pswd password shutdown --nodes 192.168.0.220 192.168.0.221 192.168.0.222

    or with IP range syntax ".."

        jdss-api-tools.exe --pswd password shutdown --node 192.168.0.220..222

13. <b>Reboot</b> single JovianDSS server.

        jdss-api-tools.exe reboot --node 192.168.0.220

14. <b>Set host name</b> to "node220", server name to "server220" and server description to "jdss220".

        jdss-api-tools.exe set_host --host node220 --server server220 --description jdss220 --node 192.168.0.220

15. <b>Set timezone and NTP-time</b> with default NTP servers.

        jdss-api-tools.exe set_time --timezone America/New_York --node 192.168.0.220
        jdss-api-tools.exe set_time --timezone America/Chicago --node 192.168.0.220
        jdss-api-tools.exe set_time --timezone America/Los_Angeles --node 192.168.0.220
        jdss-api-tools.exe set_time --timezone Europe/Berlin --node 192.168.0.220

16. <b>Set new IP settings</b> for eth0 and set gateway-IP and set eth0 as default gateway.

    Missing netmask option will set default 255.255.255.0

        jdss-api-tools.exe network --nic eth0 --new_ip 192.168.0.80 --new_gw 192.168.0.1 --node 192.168.0.220

    Setting new DNS only.

        jdss-api-tools.exe network --new_dns 192.168.0.1 --node 192.168.0.220

    Setting new gateway only. The default gateway will be set automatically.

        jdss-api-tools.exe network --nic eth0 --new_gw 192.168.0.1 --node 192.168.0.220

17. <b>Create bond</b> examples. Bond types: balance-rr, active-backup.
    Default   active-backup

        jdss-api-tools.exe create_bond --bond_nics eth0 eth1 --new_ip 192.168.0.80 --node 192.168.0.80
        jdss-api-tools.exe create_bond --bond_nics eth0 eth1 --new_ip 192.168.0.80 --new_gw 192.168.0.1 --node 192.168.0.80
        jdss-api-tools.exe create_bond --bond_nics eth0 eth1 --bond_type active-backup --new_ip 192.168.0.80 --new_gw 192.168.0.1 --node 192.168.0.80

18. <b>Delete bond</b>.

        jdss-api-tools.exe delete_bond --nic bond0 --node 192.168.0.80

19. <b>Bind cluster</b>. Bind node-b: 192.168.0.81 with node-a: 192.168.0.80

    RESTapi user   admin, RESTapi password   password, node-b GUI password   admin

        jdss-api-tools.exe bind_cluster --user admin --pswd password --bind_node_password admin --node 192.168.0.80 192.168.0.81

20. <b>Set HA-cluster ping nodes</b>. First IP   access node IP, next IPs are new ping nodes

    RESTapi user   administrator, RESTapi password   password, netmask   255.255.0.0

        jdss-api-tools.exe set_ping_nodes --user administrator --pswd password --netmask 255.255.0.0  --ping-nodes 192.168.0.240 192.168.0.241 192.168.0.242 --node 192.168.0.80 

    Same, but with defaults: user   admin, password   admin and netmask   255.255.255.0

        jdss-api-tools.exe set_ping_nodes  --ping-nodes 192.168.0.240 192.168.0.241 192.168.0.242 --node 192.168.0.80

21. <b>Set HA-cluster mirror path</b>. Please enter comma separated NICs, the first NIC must be from the same node as the specified access IP.

        jdss-api-tools.exe set_mirror_path --mirror_nics eth4 eth4 --node 192.168.0.82

22. <b>Create VIP (Virtual IP)</b> examples. 

        jdss-api-tools.exe create_vip --pool Pool-0 --vip_name vip21 --vip_nics eth2,eth2 --vip_ip 192.168.21.100  --vip_mask 255.255.0.0 --node 192.168.0.80
        jdss-api-tools.exe create_vip --pool Pool-0 --vip_name vip31 --vip_nics eth2      --vip_ip 192.168.31.100  --node 192.168.0.80

    If cluster is configured both vip_nics must be provided.
    With single node (no cluster) only first vip_nic specified will be used.
    The second nic (if specified) will be ignored. Default vip_mask 255.255.255.0

23. <b>Start HA-cluster</b>. Please enter first node IP address only.

        jdss-api-tools.exe start_cluster --node 192.168.0.82

24. <b>Move (failover)</b> given pool.

    The current active node of given pool will be found and pool will be moved to passive node.

        jdss-api-tools.exe move --pool Pool-0 --node 192.168.0.82

25. <b>Create storage resource</b>. Creates iSCSI target with volume or SMB share with dataset.

    iSCSI target with volume

        jdss-api-tools.exe create_storage_resource --pool Pool-0 --storage_type iscsi --volume zvol00 --target_name iqn.2018-08:ha-00.target0 --size 1TB --provisioning thin --node 192.168.0.220

    with defaults: size 1TB, provisioning thin volume auto target_name auto
    if target_name auto(default), the cluster name "ha-00" will be used in the auto-target_name. In this example target name will be: iqn.2018-09:ha-00.target000
    if iqn.2018-09:ha-00.target000 and zvol000 allreday exist program will use next one: if iqn.2018-09:ha-00.target1 and zvol001

        jdss-api-tools.exe create_storage_resource --pool Pool-0 --storage_type iscsi --cluster ha-00 --node 192.168.0.220

    with missing --cluster ha-00, it will produce same result as "ha-00" is default cluster name.

        jdss-api-tools.exe create_storage_resource --pool Pool-0 --storage_type iscsi --node 192.168.0.220
       
        jdss-api-tools.exe create_storage_resource --pool Pool-0 --storage_type smb --volume vol000 --share_name data  --node 192.168.0.220

    with defaults: volume auto share_name auto

        jdss-api-tools.exe create_storage_resource --pool Pool-0 --storage_type smb  --node 192.168.0.220

    and multi-resource with --quantity option, starting consecutive number from zero (default)

        jdss-api-tools.exe create_storage_resource --pool Pool-0 --storage_type iscsi --quantity 5  --node 192.168.0.220

    and multi-resource with --quantity option, but starting consecutive number from 5 (--start_with 10)
    
        jdss-api-tools.exe create_storage_resource --pool Pool-0 --storage_type iscsi --quantity 5 --start_with 10  --node 192.168.0.220
    

26. <b>Scrub start|stop</b>.
 
    Scrub all pools. If the node belongs to cluster, scrub all pools in cluster.

        jdss-api-tools.exe scrub 192.168.0.220

    Scrub specified pools only.
    
        jdss-api-tools.exe scrub --pool Pool-0 --node 192.168.0.220
        jdss-api-tools.exe scrub --pool Pool-0 --pool Pool-1 --pool Pool-2 --node 192.168.0.220

        jdss-api-tools.exe scrub --action stop --node 192.168.0.220
    

27. <b>Set scrub scheduler</b>.
    By default the command search all pools on node ot cluster(if configured) and set default schedule: evey month at 0:15.
    Every pool will set on diffrent month day.

        jdss-api-tools.exe set_scrub_scheduler --node 192.168.0.220

    set default schedule on Pool-0 and Pool-1 only.
    
        jdss-api-tools.exe set_scrub_scheduler  --pool Pool-0 Pool-1 --node 192.168.0.220

    set chedule on every week on Monday at 1:10 AM on Pool-0 only.
    
        jdss-api-tools.exe set_scrub_scheduler  --pool Pool-0 --day_of_the_month * --day_of_the_week 1 --hour 1 --minute 10 --node 192.168.0.220

    set chedule on every day at 2:30 AM on Pool-0 only.
    
        jdss-api-tools.exe set_scrub_scheduler  --pool Pool-0 --day_of_the_month * --hour 2 --minute 30 --node 192.168.0.220

    set chedule on every second day at 21:00 (9:00 PM))on Pool-0 only.
    
        jdss-api-tools.exe set_scrub_scheduler  --pool Pool-0 --day_of_the_month */2 --hour 20 --minute 0 --node 192.168.0.220

    <b>TIP:</b>
    Quick schedule params check via browser on  Pool-0 192.168.0.220:
    https:// 192.168.0.220:82/api/v3/pools/ Pool-0/scrub/scheduler


28. <b>Genarate factory setup files for batch setup.</b>.
    It creates and overwrite(if previously created) batch setup files.
    Setup files need to be edited and changed to required setup accordingly.
    For single node setup single node ip address can be specified.
    For cluster, both cluster nodes, so it will create setup file for every node.

        jdss-api-tools.exe create_factory_setup_files --nodes 192.168.0.80 192.168.0.81


29. <b>Execute factory setup files for batch setup.</b>.
     This example run setup for nodes 192.168.0.80, 192.168.0.81.
     Both nodes nned to be fresh rebooted with factory defaults eth0=192.168.0.220.
     First only one node must be started. Once booted, the REST api must be enabled via GUI.
     The batch setup will start to cofigure first node. Now, the second node can be booted.
     Once the second node is up, also the REST api must be enabled via GUI.


        jdss-api-tools.exe batch_setup  --setup_files  api_setup_single_node_80.txt api_setup_single_node_81.txt api_setup_cluster_80.txt api_test_cluster_80.txt  --node 192.168.0.80


30. <b>Print system info</b>.

        jdss-api-tools.exe info --node 192.168.0.220


############################################################################################

After any modifications of source jdss-tools.py, run pyinstaller to create new jdss-tools.exe:

	C:\Python27\Scripts>pyinstaller.exe --onefile jdss-api-tools.py

And try it:

	C:\Python27>dist\jdss-api-tools.exe -h

Missing Python modules need to be installed with pip, e.g.:

	C:\Python27\Scripts>pip install ipcalc
	C:\Python27\Scripts>pip install ping
	C:\Python27\Scripts>pip install colorama


NOTE:
In case of error: "msvcr100.dll missing...",
download and install "Microsoft Visual C++ 2010 Redistributable Package (x86)": vcredist_x86.exe
