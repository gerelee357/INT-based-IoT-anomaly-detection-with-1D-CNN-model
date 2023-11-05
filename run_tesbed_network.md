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
	
  	**Note:**  To initiate a fresh run, clear all existing data and configurations by executing the following command.
			sudo mn -c


**7. Launch the database and INT collector**

	- Start influx database process 
 
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
  
  	**Note:** If you can not see INT data in the influx database, reconfigure INT intent. 

**10. Generate sample flow to monitor on Mininet.**

	For example, you send UDP or TCP flow between h1 and h7 hosts.

 	1. Start UDP server on host h7:
  
 		iperf -s -u -i 1 
   
	2. Initiate UDP traffic from h1 to h7:
  
		h1 iperf -c h7 -u -t 100000 -l 2000B
 
		initiates UDP traffic from h1 to h7 with a duration of 100,000 seconds (-t 100000) and a packet size of 2000 bytes (-l 2000).


**11. Start grafana**
      
	sudo systemctl start grafana-server
	sudo systemctl status grafana-server
 
 **12.To connect an InfluxDB (INTdatabase) to Grafana and add queries to fetch data, follow these steps:** 
 
 	
	1. Login to Grafana:
		Open your web browser and go to http://localhost:3000
		Log in to Grafana using your credentials.
	2. Add InfluxDB Data Source:
		Click on the gear icon (⚙️) in the left sidebar to open the "Configuration" menu.
		Select "Data Sources."
		Click on the "Add your first data source" or the "+ Add" button.

    Configure InfluxDB Data Source:
    
		Choose "InfluxDB" from the list of available data sources.
		In the "HTTP" section:
			URL: Enter the URL of your InfluxDB instance (e.g., http://localhost:8086).
			Access: Choose "Server" to have Grafana access InfluxDB directly.
		In the "InfluxDB Details" section:
			Database: Enter the name of your InfluxDB database.
			User: Enter your InfluxDB username.
			Password: Enter your InfluxDB password.
		Click "Save & Test" to verify the connection to the InfluxDB database.

 	 3. Add Queries to Get Data from INT Database:
		Create a new dashboard or open an existing one.
		Click on the "+" icon in the upper-left corner to add a new panel.
		In the panel settings, click on the "Query" section.
		Example Query:
			Select the appropriate measurement and field from your INT database.

		Use the query editor to construct your query. For example:
  			SELECT field_name FROM flow_hop_latency,10.0.0.1:39513->10.0.0.7:5001,proto=17,sw_id=11 WHERE time > now() - 1h

     		This query selects data from the last 1 hour. Adjust the time range and query according to your specific use case.

		Click on "Apply" to apply the query.

		You can further customize visualization options, such as choosing a graph type, legend format, and more.

	4. Save and View Dashboard:
	Once you have configured your queries and visualizations, click on the disk icon (💾) to save the dashboard.
	Give your dashboard a name and click "Save."
	Now, your Grafana dashboard is connected to the INT database, and it displays data based on your queries. Make sure to tailor the queries and visualizations to your specific data and 	requirements.
     

 
