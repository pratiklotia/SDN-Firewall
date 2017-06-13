# SDN-Firewall
Layer 4 Firewall for Software Defined Networks (SDN)

Software Defined Network based Layer 4 Firewall based on Open Flow protocol. Securing the SDN controller is critical to the security of the entire SDN.

Firewall: A firewall is a very critical application for any network. It acts as the first/last line of defense against any unauthorized user trying to access or exploit the network. Such users can cause several harm to the networks by adding/changing flow entries to cause misconfigurations, execute DDoS attacks or just silently sniff critical information of the network. The purpose of this project is to implement a simple Layer 4 firewall on Ryu controller using a python code and understand how rules are implemented to execute certain actions on the packets which match them. The experience gained from understanding this lab should be used as a foundation to understanding higher layer firewalls and how software can control the network. In the future, this basic firewall can be enhanced with additional functionality.

Software Defined Network (SDN):

SDN is a networking concept that aims to centralize networks and make network flows programmable, and NFV focusses on virtualized network functions. SDNFV can be used to manage networks better and reduce CapEx/ OpEx. SDN-based load-balancers use SDNFV functions and applications to create flexible, programmable, and virtual load-balancing that can be deployed, managed and manipulated with ease in the industry.

This is a stateless round robin load balancer designed to be executed on Control Plane of SDN. It uses Open Flow protocol. This is fully tested on Mininet VM. The Controller supported is Ryu controller (https://osrg.github.io/ryu/) which is a Python based SDN controller.

OpenFlow: OpenFlow® is the first standard communications interface defined between the control and forwarding layers of an SDN architecture. OpenFlow® allows direct access to and manipulation of the forwarding plane of network devices such as switches and routers, both physical and virtual (hypervisor-based). OpenFlow-based SDN technologies enable IT to address the high-bandwidth, dynamic nature of today's applications, adapt the network to ever-changing business needs, and significantly reduce operations and management complexity. For historical information about the origins of OpenFlow® at Stanford University prior to the creation of ONF, please see archive.openflow.org.

Ryu: Ryu is a component-based software defined networking framework. Ryu provides software components with well defined API that make it easy for developers to create new network management and control applications. Ryu supports various protocols for managing network devices, such as OpenFlow, Netconf, OF-config, etc. About OpenFlow, Ryu supports fully 1.0, 1.2, 1.3, 1.4, 1.5 and Nicira Extensions. All of the code is freely available under the Apache 2.0 license.

Mininet: Mininet creates a realistic virtual network, running real kernel, switch and application code, on a single machine (VM, cloud or native), in seconds, with a single command. Because you can easily interact with your network using the Mininet CLI (and API), customize it, share it with others, or deploy it on real hardware, Mininet is useful for development, teaching, and research. Mininet is also a great way to develop, share, and experiment with OpenFlow and Software-Defined Networking systems. Mininet is actively developed and supported, and is released under a permissive BSD Open Source license.



Task:
Layer 4 firewall using Ryu as your controller. This firewall should allow TCP traffic based on 4 tuples (src_ip, dst_ip, src_port, dst_port) that you provide as an input via a csv or a config file pre-defined within the firewall application. Allowing only TCP traffic is a condition you should not overlook. To modify the firewall rules, the config file has to be edited and the ryu firewall app has to be restarted. The firewall app code does not have to be edited for a change in firewall rules. Only tcp should be blocked, udp and icmp should be able to pass through.


Solution:

Procedure to run the code: (please ensure internet connectivity and connectivity between the Ryu & Mininet VM before starting the procedure).
1. Paste the configuration file named ‘Allowance.csv’ in the Ryu VM’s /home/sdn/ directory.
2. Start the Ryu Controller along with the OneFirewall.py program (Please note the IP address of this VM beforehand – using ‘ifconfig’). The OneFirewall.py program must be stored at /usr/local/lib/python2.7/dist-packages/ryu/app/
This is same location where in-built programs like SimpleSwitch.py are stored.
sudo ryu run OneFirewall.py --observe-links --verbose
3. Create a simple Mininet topology, preferably using one switch and few hosts.
Eg. sudo mn --controller=remote,ip=<ipaddr> --mac --switch ovs,protocols=OpenFlow13 --topo single,7 --ipbase=10.0.0.1/24
Where <ipaddr> is the IP address of your Ryu controller such as ‘192.168.56.111’.
4 Execute ‘pingall’ from the Mininet shell.
5 The configuration file (Allowance.csv) is configured to allow TCP traffic between HTTP port of host 1 (having IP Address 10.0.0.1) & host 2 (IP Address 10.0.0.2) and between HTTP port of host 1 & host 3 (IP Address 10.0.0.3).
6 The program can be tested by starting a HTTP server on host 1 with the command ‘h1 python –m SimpleHTTPServer 80 &’. Next execute ‘h2 wget h1’. You will see that h2 client receives HTTP traffic. Similarly, h3 client is also able to have HTTP traffic from host 1.
7 Next try executing ‘h4 wget h1’. It will try to connect but will fail to establish any connection as
Lab 6: SDN Security 15
the Firewall rules do not permit it. Similarly any communication between other HTTP servers and clients shall also be blocked.
8 If you are trying to alter the Allowance.csv file, please ensure that you follow the format.




I encourage you to contribute code, bug reports/fixes, documentation, and anything else that can improve the system! For any queries, please email me at pratiklotia@yahoo.in
