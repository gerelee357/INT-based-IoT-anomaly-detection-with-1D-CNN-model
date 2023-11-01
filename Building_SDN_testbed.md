# Building SDN testbed with P4 virtual switch and ONOS network controller

**1. Operating system requirement**

	- Ubuntu 18.04: 
	- Kernel version ( check it by using 'uname -r' command )    4.15.0-166-generic


**2. Download ONOS-2.4.0 controller.** 

 	Download the following commit published on Jun 4, 2020, because this version supports INT GUI. 
	https://github.com/opennetworkinglab/onos/tree/555617a9f918209cfd6b2c283d6a80638f83c17a
	
**3. Once ONOS has been downloaded, extract the files and rename the extracted folder to 'onos'. Next, navigate to the 'onos' directory and read the README.md file.  Then, install the necessary packages required for constructing the SDN network with P4 by executing the following script.**

	cd onos/tools/dev/p4vm
	./install-p4-tools.sh 

**3. Before you proceed with the installation of ONOS, it's necessary to install Bazel, which is required for compiling ONOS. Add the Bazel distribution URL as a package source before installing it.**

	echo "deb [arch=amd64] https://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
	curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -

	sudo apt-get update && sudo apt-get install bazel-1.0.0


**5. Build ONOS by using basel.** 

    
    cd onos
	sudo bazel-1.0.0 build onos
	
    

**6.Run ONOS.**

	cd onos
	ONOS_APPS=drivers.bmv2,proxyarp,lldpprovider,hostprovider,fwd 
	sudo bazel-1.0.0 run onos-local -- clean 

**7. When running the above lines, it gives an error. To fix it, run following.(it is error because if 4.15.0-166-generic kernel )**

	ssh-keygen -t rsa 
 	~/.ssh/rsa-key gesen 


**8. Install Mininet 2.3.0.**


	cd ~
        git clone https://github.com/mininet/mininet  # if it's not already there
	cd mininet
	git checkout 2.3.0
	HEAD is now at d7f399d 2.3.0 (#1043)

	/util/install.sh -w
	mn --version
		2.3.0

**9. To install BCC and INT collector, please use following guidline.** 

- Install_BCC_and_INT_collector.md


**10. Add virtual interfaces for the INT collector.**

	sudo ip link add veth_1 type veth peer name veth_2 
	sudo ip link set dev veth_1 up 
	sudo ip link set dev veth_2 up 

**11. Connect INT Collector with the virtual interface by modifying the file $ONOS_ROOT/tools/test/topos/bmv2-demo.py.**


Look for this line: 

	from mininet.link import TCLink
 
Change the line as follows:

	from mininet.link import TCLink, Intf
 

Look for this line: 
	net.build()

Below this line, add the following:
	collectorIntf = Intf( 'veth_1', node=net.nameToNode[ "s12" ] )

**12. Install Grafana using the following instructions.**

https://grafana.com/grafana/download/5.4.2?platform=linux

**13. Grafana configuration.** 

Settings-> data source 

influxdb 

http://localhost:8086  

INTdatabase

**14. Once you've configured the INT database in Grafana, launch Grafana, and you'll be able to visualize the graphics representing the INT data.**










