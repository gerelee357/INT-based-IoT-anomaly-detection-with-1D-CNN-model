**1. Add virtual interfaces for the collector.**

	sudo ip link add veth_1 type veth peer name veth_2
        sudo ip link set dev veth_1 up 
        sudo ip link set dev veth_2 up

 **Note:** You can check virtual interfaces by using ifconfig command. 



**2. Run ONOS controller**

	cd onos

	ONOS_APPS=drivers.bmv2,proxyarp,lldpprovider,hostprovider,fwd 
	sudo bazel-1.0.0 run onos-local -- clean 

**Note:** You can manage ONOS controller via GUI or CLI. 

**3. Login  to ONOS GUI:**
 
 		http://localhost:8181/onos/ui/login.html
			username: karaf
			password: karaf
   
**4. Activate the following apps from ONOS GUI**

 	- Go to the applications menu and activate the following one by one. 

	org.onosproject.fwd 
	org.onosproject.drivers.bmv2
	org.onosproject.proxyarp
	org.onosproject.lldpprovider
	org.onosproject.hostprovider

 	- Or You can activate the using ONOS CLI 
  
 	app activate org.onosproject.fwd 
	app activate org.onosproject.drivers.bmv2
	app activate org.onosproject.proxyarp
	app activate org.onosproject.lldpprovider
	app activate org.onosproject.hostprovider

**5. Setup ONOS ENV variables**

	export ONOS_ROOT=/home/gerel/onos
	export RUN_PACK_PATH=~/onos/tools/package/runtime/bin


**6. Run SDN network with ONOS controller and P4 switches(BMv2) in MININET.** 



	$ sudo -E $ONOS_ROOT/tools/test/topos/bmv2-demo.py --onos-ip=127.0.0.1 --pipeconf-id=org.onosproject.pipelines.int
	
  	To initiate a fresh run, clear all existing data and configurations by executing the following command.
  
		sudo mn -c


**7. Launch the database and INT collector**

	- Start influxdb database process 
 
		$ sudo systemctl start influxdb
  
  	- Start BPF collector
		$ sudo python3 BPFCollector/InDBClient.py veth_2


**8. Configure mirror port on s12 for sending INT data to INT collector**

	simple_switch_CLI --thrift-port `cat /tmp/bmv2-s12-thrift-port`
	mirroring_add 500 4

  	**Note.** 4 can be different. You can check it by using the ports command in Mininet. 
		

**9. Manage INT services on ONOS GUI**

	You can check the following link as a reference for this. 
	https://www.youtube.com/watch?v=ZXRef0IhXGM
 
 	-  Enable INT services  
 		Navigate applications, search inband, and activate it.
  
	- Then, navigate to In-band Telemetry control, and configure the INT report collector
		address: 127.0.0.1 
		port: 54321

	- Then, you can configure INT intent. What traffic do you want to capture? 
 		For example, 

		Src address: 10.0.0.1
		Dst address: 10.0.0.5
		Src port: 
		Dst port: 5001 
		Protocol: UDP

**10. Generate sample flow to monitor on mininet.**

	for example, sending UDP or TCP flow between hosts.
 
	h1 iperf -c h5 -u -t 100000 -l 2000B
 
		-t 100000
		-l 250B


**11.Start grafana**

        
	sudo systemctl start grafana-server
	sudo systemctl status grafana-server
