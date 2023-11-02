**1. Add virtual interfaces for the collector.**

	sudo ip link add veth_1 type veth peer name veth_2
        sudo ip link set dev veth_1 up 
        sudo ip link set dev veth_2 up

 **Note:** you can check virtual interfaces by using ifconfig command. 



**2. Run ONOS**

	cd onos

	ONOS_APPS=drivers.bmv2,proxyarp,lldpprovider,hostprovider,fwd 
	sudo bazel-1.0.0 run onos-local -- clean 


**3. Activate apps from CLI or GUI**
	http://localhost:8181/onos/ui/login.html
	username: karaf
	password: karaf

	onos> app activate org.onosproject.fwd 
	app activate org.onosproject.drivers.bmv2
	app activate org.onosproject.proxyarp
	app activate org.onosproject.lldpprovider
	app activate org.onosproject.hostprovider

**4. Setup ENV variables**
	export ONOS_ROOT=/home/gerel/onos
	export RUN_PACK_PATH=~/onos/tools/package/runtime/bin

 
 

**5. RUN network in MININET** 
	gerel@pc:~/onos$ sudo -E $ONOS_ROOT/tools/test/topos/bmv2-demo.py --onos-ip=127.0.0.1 --pipeconf-id=org.onosproject.pipelines.int
	
  to create a new run, clean everything using the following command. 
  
	sudo mn -c


**6.Launch the collector**

	$ sudo systemctl start influxdb
	$ sudo python3 BPFCollector/InDBClient.py veth_2

gerel@an-INT:~$ sudo python3 BPFCollector/InDBClient.py veth_2
/usr/lib/python3/dist-packages/requests/__init__.py:80: RequestsDependencyWarning: urllib3 (1.26.9) or chardet (3.0.4) doesn't match a supported version!
  RequestsDependencyWarning)


**7. Configure mirror port**
	simple_switch_CLI --thrift-port `cat /tmp/bmv2-s12-thrift-port`
	mirroring_add 500 4
		4 gedeg port veth_1 gedeg interface-tei holbogson esehiig mininet-ees ports gesen command-r harna

**8. ENABLE INT services**
	enable INT
	https://www.youtube.com/watch?v=ZXRef0IhXGM
	configure INT report collector
		address: 127.0.0.1 
		port: 54321
		

**9. Add INT Intent on ONOS GUI**

	Src address: 10.0.0.1
	Dst address: 10.0.0.2
	Src port: 
	Dst port: 5001 
	Protocol: UDP

**10. Generate sample flow to monitor.**

	send udp or tcp flow between hosts
	h1 iperf -c h5 -u -t 100000 -l 2000B
		-t 100000
		-l 250B


**11.Start grafana**

        
	sudo systemctl start grafana-server
	sudo systemctl status grafana-server
