
1. INT collector-iig ajilluulahaas omno. BCC gesen kernel module-iig zaaval ajilluulah yostoi. 

2. Install bcc

Download bcc kernel module: 
	bcc-g source code-oos suulgah zaavar: Compiler for BPD	
	https://github.com/iovisor/bcc/blob/master/INSTALL.md#ubuntu---source
	Deerh commit oorchlogdsonoos bolood suulgaj bolohgui baisan. 
	Harin ooriin com dotor huuchin huvilbariig tataj avsan(Package folder-t baigaa) Tuuniig suulgaval asuudalgui ajillna. 

Install prerequiste packages on Ubuntu 18.04 LTS
	# For Bionic (18.04 LTS)
	sudo apt-get -y install bison build-essential cmake flex git libedit-dev \
  	libllvm6.0 llvm-6.0-dev libclang-6.0-dev python zlib1g-dev libelf-dev libfl-dev python3-distutils

Install and compile BCC

	git clone https://github.com/iovisor/bcc.git  (ooriin com deerh huvilbariig suulga)
	mkdir bcc/build; cd bcc/build
	cmake ..
	make
	sudo make install
	cmake -DPYTHON_CMD=python3 .. # build python3 binding
	pushd src/python/
	make
	sudo make install
	popd


2. Install and run INT collector (https://wiki.onosproject.org/display/ONOS/In-band+Network+Telemetry+%28INT%29+with+ONOS+and+P4)

	git clone https://gitlab.com/tunv_ebpf/BPFCollector.git
	cd BPFCollector
	git checkout -t origin/spec_1.0

Enable JIT for eBPF to make BPFCollector run faster
	sudo sysctl net/core/bpf_jit_enable=1

Install required python modules
	sudo pip3 install prometheus_client
	sudo pip3 install influxdb
	sudo pip3 install cython 

install influx

	wget https://dl.influxdata.com/influxdb/releases/influxdb_1.2.4_amd64.deb
	sudo dpkg -i influxdb_1.2.4_amd64.deb

Run INT collector
	sudo systemctl start influxdb
	sudo python3 BPFCollector/InDBClient.py veth_2


Finish






*****************************************************8

NTCollector use 
	eBPF and XDP, which require recent linux kernel. For best practice, kernel version >= v4.14 should be used.


	Berkeley Packet Filters (BPF) provide a powerful tool for intrusion detection analysis. 
	Use BPF filtering to quickly reduce large packet captures to a reduced set of results by
        filtering based on a specific type of traffic. Both admin and non-admin users can create BPF filters.

	eBPF is an extended BPF JIT virtual machine in the Linux kernel. 

XDP

	XDP (eXpress Data Path) is an eBPF-based high-performance data path merged in the Linux kernel since version 4.8.[1] 




Run example

	root@gerel-VirtualBoxpc:/home/gerel/bcc/examples# python3 hello_world.py
	b'            bash-6756  [000] ....  5558.255279: 0x00000001: Hello, World!'
	b'            bash-6756  [000] ....  5558.255291: 0x00000001: Hello, World!'
	b'            bash-6756  [000] ....  5558.255292: 0x00000001: Hello, World!'
	b'            bash-6756  [000] ....  5560.739052: 0x00000001: Hello, World!'
	b'            bash-6756  [000] ....  5560.739054: 0x00000001: Hello, World!'
	b'            bash-6756  [000] ....  5560.739056: 0x00000001: Hello, World!'
	b'            bash-6756  [000] ....  5573.588335: 0x00000001: Hello, World!'
	b'            bash-6756  [000] ....  5573.588337: 0x00000001: Hello, World!'
	b'            bash-6756  [000] ....  5573.588338: 0x00000001: Hello, World!'
	b'            bash-6756  [000] ....  5631.457289: 0x00000001: Hello, World!'
	b'            bash-6756  [000] ....  5631.457291: 0x00000001: Hello, World!'
	b'            bash-6756  [000] ....  5631.457293: 0x00000001: Hello, World!'


change kernel version

	https://ixnfo.com/en/how-to-roll-back-kernel-ubuntu.html
	install kernel update software
	https://github.com/teejee2008/ukuu/releases


bcc-tei holbootoi zarim neg useful resource

	https://sky-x.blog.csdn.net/article/details/106783149?spm=1001.2101.3001.6650.17&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-17.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-17.pc_relevant_default&utm_relevant_index=23
	https://www.cnblogs.com/xzkzzz/p/9627658.html




