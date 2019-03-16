# -DNSSEC
Domain Name System (DNS) is a hierarchical naming system built on distributed database. This system translates domain name to IP address or vice-versa and make it possible to assign domain name to group of internet resources and users. DNS protocol uses port 53 of UDP transport layer protocol which does not contain security by itself and does not have foolproof built in authentication making it more susceptible to hacking than other network based services. Domain Name System Security Extension (DNSSEC) are a collection of protocols that add a layer of security to the domain Name System lookup and exchange processes, which have become integral in accessing websites through the internet. It is set of extensions to DNS which provides DNS clients authentications of DNS data. The motivation for doing this project is primarily an interest in undertaking challenging project in the field of DNSSEC because during the part of the course we found the topic very interesting and hence wanted to pursue our project on this topic so that we could acquire more knowledge and experiences in this concept. In this project we  installed and configured DNS Test Bed Hierarchy by configuring Root, TLD, STLD DNS Servers for forward and reverse lookups zones. We done the evaluation on this hierarchy to check DNS query latency from the identified vantage point.  We implemented DNSSEC on DNS Test Bed Hierarchy and again done the evaluation for DNS query latency, and done the analysis to compare the query latency with DNSSEC and without DNSSEC.


1 INTRODUCTION
The Domain Name System (DNS) is the Internet naming service. Its functionality is mapping domain names to IP addresses and IP addresses to domain names . Nowadays DNS becomes very crucial that almost all Internet applications are using it. So it is more necessary to secure and enhance it. Unfortunately, as the DNS protocol is very old, it has some vulnerabilities suchas lack of authentication and data integrity. These weaknesses enable attackers to poison the caches of name servers and inject fake data. So what is the solution?
DNS Security Extensions or DNSsec can help us in these situations to reduce the compromise of the DNS infrastructure. DNSSEC was created by IETF to meet the DNS requirements. Each zone creates public/private key pairs to sign its data. This brings us authentication and integrity checking. The private key is held by the administrator and kept secret while the public key is added to the zone in a resource record called a DNSKEY record.
And we create a DNS Reverse and Forward Hierarchy.







 
                                                  Fig.1.1   DNS Hierarchy


2 DOMAIN NAME SYSTEM
The Domain Name System (DNS) is a hierarchical distributed naming system used for mapping domain names to IP addresses. It is fundamental and required for every transaction on Internet like visiting a website or sending an email. With DNS a machine can locate other computers worldwide. It is like a phonebook of an internet where all IP Addresses and their domain names are stored. We use Dns as we can not remember the IP Addresses. DNS works on port number 53 of UDP for query. It may also use TCP for zone transfer over port 53 and UDP for DNS query over port 53. Eg., “www.cdac.in” 192.168.1.14 IP Addresses. DNS can be also called as NameServer. DNS was introduced in 1980s, and since then it has successfully serving the need of internet users, despite the phenomenal increase in its database.


2.1 Domain Resolution

Each domain name consists of two or more sub-domain names separated by “.”. The information that these sub-domains provide is distributed around the world in the hierarchical matter.
There are 13 root servers, mirrored by many more around the world which can provide the IP address of the lower level domain. The top level domains consist of the gTLDs (com, net, org, gov) and the ccTLDs (nl, gr, uk) which can provide the IP address of the domains they have delegation for. Moreover, each administrator that registers a domain name, assigns IP addresses for its services (web, email) which are subsequently requested by the users at the last step.
Domain names are synonymous to DNS zones except that in the DNS zones administrative responsibility has been delegated to a single manager. As a result, a domain name can be partitioned in different sub-levels which are managed independently. A DNS zone is implemented in the configuration system of a domain name server in the form of a zone file containing all the required information needed for a domain to operate properly and provide the requested information.
DNS primarily uses User Datagram Protocol (UDP) on port number 53 to serve requests. DNS queries consist of a single UDP request from the client followed by a single UDP reply from the server. The Transmission Control Protocol (TCP) is used in some cases when the response data size exceeds 512 bytes, or for tasks such as zone transfers.

2.2 DNS main components

DNS is built based on some basic architecture components needed from the time a user makes a query, which is propagated through these components to its final destination, all the way back to the time he receives his answer. These components are:

2.2.1 Stub Resolver
Stub resolver lies on the client side. It is responsible for initiating the queries that ultimately lead to a full resolution of a domain. It usually contacts and relies on a recursive resolver that will propagate the queries needed to resolve the requested domains and receive the answers for it.

2.2.2 Recursive Resolver
Recursive resolver is the intermediate component between the Stub Resolver belonging to the client side and the Authoritative Nameservers belonging to the server side where the domains are configured. As a result, it is the component that resolves all the sub-domains that compose the initial domain that client queried for. To achieve that, it contacts each sub-domain’s nameserver starting from the root and ending to the final domain. Recursive resolver has the option to caches the answer it receives from each nameserver (Caching Resolver) for the time (TTL) provided by the nameserver. Using caches, improves efficiency, increases performance in end-user applications by returning faster answers and decreases the operating burden on the queried servers. ISP and large organisations provide recursive/caching resolvers to their users in case they have not configured one on their own.

2.2.3 Authoritative Nameserver
Each domain including root should contain at least two authoritative nameservers for better functionality and protection against cases of server failure and inability to respond to potential requests. An authoritative nameserver has the permission to return answers for each query comes to the server, related to a domain configured by the administrator. Each authoritative nameserver of a domain must return the consistent answer, in the order configured by the administrator. The return of the same answer from all authoritative nameservers of a domain is not always the case as we will present afterwards.

2.3 Resource Records

A resource record (RR) is the basic data element in the domain name system. It is contained in each domain zone and is returned as an answer to the DNS component that request it.
The attributes of a resource record are:

2.3.1 Owner - Fully qualified domain name (FQDN) that this RR belongs.

2.3.2 TTL - Time to live in the cache.

2.3.3 Class - usually IN, Internet.

2.3.4 Type - For what purpose it will be used such as A, AAAA, MX, CNAME, PTR, SOA, NS, DNAME and SRV.

●	A -The record A specifies IP address (IPv4) for given host. A records are used for conversion of domain names to corresponding IP addresses. It is returns a 32-bit IP address.

●	 AAAA -The record AAAA (also quad-A record) specifies IPv6 address for given host. So it works the same way as the A record and the difference is the type of IP address. It is returns a 128-bit IP address.

●	CNAME - The CNAME record specifies a domain name that has to be queried in order to resolve the original DNS query. Therefore CNAME records are used for creating aliases of domain names. CNAME records are truly useful when we want to alias our domain to an external domain. In other cases we can remove CNAME records and replace them with A records and even decrease performance overhead.

●	DNAME - A DNAME record is used for non-terminal DNS Name Redirection.

●	MX - The MX resource record specifies a mail exchange server for a DNS domain name. The information is used by Simple Mail Transfer Protocol (SMTP) to route emails to proper hosts. Typically, there are more than one mail exchange server for a DNS domain and each of them have set priority.

●	NS - The NS record specifies an authoritative name server for given host.

●	PTR - A PTR record is used for reverse lookup. Reverse lookup translates an IP to a domain.

●	SOA - The record specifies core information about a DNS zone, including the primary name server, the email of the domain administrator, the domain serial number, and several timers relating to refreshing the zone.

●	SRV - A service record specifies the location of the services that a domain supports.

2.3.5 RData - Data related to the RR type.

2.4 Message Format
There are two types of DNS messages, queries and replies, and they both have the same format. Each message consists of a header and four sections each containing the related information followed by the TTL, class, type and data:
• Question: Contains the queried name.
• Answer: Contains the answer to the query.
• Authority: Contains the nameservers of the domain and other referral information .
• Additional: It usually contains data related to information contained in the Authority sections.

2.5 Types DNS
 There are two types of DNS are as follows:
                  
 2.5.1 Forward DNS working

 
                                        Fig.2.5.1  Forward Lookup

When client enters Domain name into the browser for example :- www.cdac.in it first sends the request to the Recursive cache DNS. Here Recursive cache DNS checks in the cache weather the Domain name IP address is present in the cache or not. If present it will directly send back to the user/client  and the web page will be opened/displayed onto the browser.
But if the IP address is not available in the Recursive cache memory then  Recursive cache DNS server will send the request or query as www.cdac.in to the root(.) server where all 13 root name servers are available. The root server checks the query in the database. Actually It does not know the ip address of that domain name but it know where to send the Recursive cache DNS server for helping it to find the ip address of domain name so it sends referral ip address for .in next level which is called TLD server to recursive resolver. The Recursive cache DNS server again sends a query to a .in where all the web sites with .in are available, it is a first TLD (Top Level Domain) as Recursive DNS cache server ask the IP address of www.cdac.in. The TLD again checks in the database about the information. It also does not know ip address of the domain name but it knows ip address of next server. so it sends referral ip address for cdac.in next level sever.      
	Then again Recursive DNS cache server sends a query as www.cdac.in to the second top level domain. The STLD again checks in data the database about the information. It also does not know ip address of the domain name but it knows ip address of next server. So it sends referral ip address for www.cdac.in. to the Recursive cache DNS server. Then Recursive cache DNS server sends the ip address of the www.cdac.in. to the client. And client goes to that ip address and find the domain name and web page is displayed onto the web browser.
When next time client visit the same Domain name then it will be directly communicated to the cache. 





2.5.2 Reverse DNS working 
 
                                                Fig.2.5.2 Reverse Lookup

When client enters IP address into the browser for example :- 14.139.152.56 it first sends the request to the Recursive cache DNS. Here Recursive cache DNS checks in the cache weather the IP address of Domain name is present in the cache or not. If present it will directly send back to the user/client  and the web page will be opened/displayed onto the browser.
But if the Domain name is not available in the Recursive cache memory then  Recursive cache DNS server will send the request or query as 14.139.152.in-addr.arpa to the root(.) server where all 13 root name servers are available. The root server checks the query in the database. Actually It does not know the Domain name of that IP address but it know where to send the Recursive cache DNS server for helping it to find the Domain name of IP address so it sends referral ip address for arpa next level which is called TLD server to recursive resolver. The Recursive cache DNS server again sends a query to a arpa where all the web sites with arpa are available, it is a first TLD (Top Level Domain) as Recursive DNS cache server ask the Domain name of 14.139.152.in-addr.arpa The TLD again checks in the database about the information. It also does not know Domain name of the IP address but it knows ip address of next server. so it sends referral ip address for in-addr.arpa next level sever.      
	Then again Recursive DNS cache server sends a query as 14.139.153.in-addr.arpa to the second top level domain. The STLD again checks in  the database about the information. It also does not know Domain name of the IP address but it knows IP address of next server. So it sends referral IP address for 14.139.152.in-addr.arpa next level server.
Then again Recursive DNS cache server sends a query as 14.139.152.in-addr.arpa to the thied top level domain. The TTLD again checks in  the database about the information. It sends the Domain name to the Recursive cache DNS server. Then Recursive cache DNS server sends the Domain name of  14.139.152.in-addr.arpa.  And web page is displayed onto the web browser.
When next time client visit the same IP address then it will be directly communicated to the cache.

2.6 DNS Attacks

 2.6.1. Man in the middle attack :- Man-in-the-middle attacks are a common type of cybersecurity attack that allows attackers to over on the communication between two targets. The attack takes place in between two legitimately communicating hosts, allowing the attacker to “listen” to a conversation they should normally not be able to listen to, hence the name “man-in-the-middle.”

Here’s an analogy: Alice and Bob are having a conversation; Eve wants to eavesdrop on the conversation but also remain transparent. Eve could tell Alice that she was Bob and tell Bob that she was Alice. This would lead Alice to believe she’s speaking to Bob, while actually revealing her part of the conversation to Eve. Eve could then gather information from this, alter the response,  and pass the message along to Bob (who thinks he’s talking to Alice). As a result, Eve is able to transparently hijack their conversation.

 
Fig. 3 DNS Attacks

 2.6.2. Cache Poisoning attack :- cache poisoning is DNS cache poisoning where attackers replace a genuine IP address in the DNS cache with an IP address they control. Unaware of what has happened, users served from this DNS cache are redirected to the attacker’s compromised site. 

 2.6.3. Modified data attack :- After an attacker has seen and read your data, the next logical step he will most probably take is altering it.
An attacker can modify the data without the knowledge of the sender or receiver. Even if you do not require confidentiality for all communications, you do not want any of your messages to be modified in transit. For example, if you are exchanging purchase requisitions, you do not want the items, amounts, or billing information to be modified.

 2.6.4. Spoofing master attack :- In spoofing master attack, the attacker masquerades as a slave DNS server to the master DNS server and gets a copy of the critical zone files can display a lot about the topology of the internal network and can be used to target specific nodes or a subnet.  

 2.6.5. Spoofed updates attack :- Dynamic updates is a feature of authoritative DNS servers, where in the zone files can be updated dynamically even from remote locations. This is secured by configuring the IP address from which the update will be initiated.

 2.6.6. Corrupted data attack :- If an attacker can corrupt the zone file of an authoritative name sever with fake resource records or negative responses. It may cause the DNS name resolution process to fail. For example, an incorrect NS or A record for “www.cdac.in” zone will make the domain unrechable or may redirect the client.  

 2.6.7. NX donaim attack :- The attack mainly targets to recursive DNS server or authoritative DNS server the attacker sends the flood of queries to these DNS server for resolving of non existing domain name. DNS server cannot find the answer of the query and reply back with NXDOMAIN results which slows down the response time for the legitimate user. If high volume flood of queries generated then cache fills up very quickly, and the legitimated user feels high delay for their responses.

 2.6.8.Against the take over of an authoritative server :- In this attack attacker directly attack to the zone file of an authoritative server. And it gives the wrong resource record for something crime by take the the handle of an authoritative server. When our recursive server will go to the stld server for find the ip address of domain name it gives the wrong resource record by the zone file which is created by the attacker.

 2.6.9.Rouge secondaries Attack :- In this attack attacker attacks the secondary server or slave server and change the data of Zone file from the slave server. That attacks happen if don’t operate your secondary yourself but you have service partner that run’s your secondaries for you. You are assume that service partner does not make any change to your zone content and it is serving only the content that you have approved.

2.7 Implementation of Forward DNS  3 level-Hierarchy testbed

In this forward DNS hierarchy we installed and configured DNS Test Bed Hierarchy by configuring Root, TLD, STLD DNS Servers for forward zone. 

2.7.1 Root Server Configuration
First we configure root server on virtual machine in 192.168.1.12 and host name is s.root-server.net.  It has 8 Gb memory. Now we configure root server stepwise. 

Step 1. Install the packages of dns.
# yum install bind bind-utils

Step 2.Create the zone file for “.” in configuration file of bind.
# vi /etc/named.conf
      Options{
                    listen -on port 53 {192.168.1.12;};
	//       listen -on -v6 port 53 {::!;};
	         Allow-query{any;};
	        
	         recursion no;
	         dnssec-enable no;
	         dnssec-validation no;
};

Zone “.” IN {
		type master;
		File “/var/named/root-zone”;
};


Step3. Create root-zone file which can stored the resources record and which can be specify the domain name IP address for next level which is called tld.
#  vi /var/named/root-zone
$TTL 1D
        	@     	IN     	SOA 	s.root-server.net. 	root.s.root-server.net.(
                                            	1  ; serial
                                            	1H    	; refresh
                                            	2H    	; retry
                                            	1W   	; expire
                                            	1H    	; minimum
        	);
        	@     	IN     	NS    	s.root-server.net.
        	s.root-server.net.  	IN     	A      	192.168.1.12
         	in.	NS     	A      ns.tld.in.
 	ns.tld.in.	A      192.168.1.13


Step4. Start the service.
# systemctl start named

2.7.2 Top Level Domain Configuration
Now we configure tld server on virtual machine in 192.168.1.13 and host name is ns.tld.in . It has 8 Gb memory. Now we configure tld server stepwise. 

Step 1. Install the packages of dns.
# yum install bind bind-utils

Step 2.Create the zone file for “in” in configuration file of bind.
# vi /etc/named.conf

      Options{
                    listen -on port 53 {192.168.1.13;};
	//       listen -on -v6 port 53 {::!;};
	         Allow-query{any;};
	        
	         recursion no;
	         dnssec-enable no;
	         dnssec-validation no;
};

Zone “.” IN {
		type hint;
		file “/var/named/named.ca”;
};

Zone “in” IN{
		type master;
		File “/var/named/zone-in”;
};

Step3. We save our root server detail in tld server.
#vi /var/named/etc/named.ca
; <<>> DiG 9.9.4-RedHat-9.9.4-38.el7_3.2 <<>> +bufsize=1200 +norec @a.root-servers.net
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 17380
;; flags: qr aa; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 27

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1472
;; QUESTION SECTION:
;.                          	IN  	NS

;; ANSWER SECTION:
.                   	518400  IN  	NS  	s.root-servers.net.

;; ADDITIONAL SECTION:
s.root-servers.net. 	3600000 IN  	A   	192.168.1.12

;; Query time: 18 msec
;; SERVER: 198.41.0.4#53(198.41.0.4)
;; WHEN: Po kvÄEST 2017
;; MSG SIZE  rcvd: 811

step4.Create root-in file which can stored the resources record and which can be specify the domain name IP address for next level which is called authoriative server.
#  vi /var/named/zone-in
$TTL 1D
        	@     	IN     	SOA 	in.	root.in. (
                                            	1  ; serial
                                            	1H    	; refresh
                                            	2H    	; retry
                                            	1W   	; expire
                                            	1H    	; minimum
        	);
        	@	IN	NS	ns.tld.in.
        	ns.tld.in.	IN	A 	192.168.1.13
         	Cdac.in.	IN	NS       ns.cdac.in.
 	ns.cdac.in.	IN	A      	192.168.1.14

Step5. Start the service.
# systemctl start named

2.7.3 Second Top Level Domain Configuration Or Authoritative Server

Now we configure stld server on virtual machine in 192.168.1.14 and host name is ns.cdac.in. It has 8 Gb memory. Now we configure stld server stepwise.  

Step 1. Install the packages of DNS.
# yum install bind bind-utils

Step 2.Create the zone file for “cdac.in” in configuration file of bind.
# vi /etc/named.conf

      Options{
                    listen -on port 53 {192.168.1.14;};
	//       listen -on -v6 port 53 {::!;};
	         Allow-query{any;}

	         recursion no;
	         dnssec-enable no;
	         dnssec-validation no;
};

Zone “.” IN {
		type hint;
		file “/var/named/named.ca”;
};
Zone “cdac.in” IN{
		type master;
		File “/var/named/zone-cdac”;
};

Step3. We save our root server detail in stld server.
#vi /var/named/etc/named.ca
; <<>> DiG 9.9.4-RedHat-9.9.4-38.el7_3.2 <<>> +bufsize=1200 +norec @a.root-servers.net
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 17380
;; flags: qr aa; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 27

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1472
;; QUESTION SECTION:
;.                          	IN  	NS

;; ANSWER SECTION:
.                   	518400  IN  	NS  	s.root-servers.net.

;; ADDITIONAL SECTION:
s.root-servers.net. 	3600000 IN  	A   	192.168.1.12

;; Query time: 18 msec
;; SERVER: 198.41.0.4#53(198.41.0.4)
;; WHEN: Po kvÄEST 2017
;; MSG SIZE  rcvd: 811


step4.Create zone-cdac file which can stored the resources record and which can be specify the domain name IP address for domain name.

#  vi /var/named/zone-in
$TTL 1D
        	@     	IN     	SOA 	cdac.in.	root.cdac.in. (
                                            	1  	; serial
                                            	1H    	; refresh
                                            	2H    	; retry
                                            	1W   	; expire
                                            	1H    	; minimum
        	);
        	@	IN	NS	ns.cdac.in.
        	ns.cdac.in.	IN	A 	192.168.1.14
         	www.cdac.in.	IN	A      192.168.1.14
mail.cdac.in.	IN	MX     	192.168.1.14

Step5. Start the service.
# systemctl start named

2.7.4.Recursive Resolver Configuration

Now we configure Recursive Resolver server on virtual machine in 192.168.1.13 and host name is recursive. It has 8 Gb memory. Now we configure resolver server stepwise.  

Step 1. Install the packages of DNS.
# yum install bind bind-utils

Step 2.Create the changes for Recursive resolver  in configuration file of bind.
# vi /etc/named.conf

      Options{
 listen -on port 53 {192.168.1.15;};
	//       listen -on -v6 port 53 {::!;};
	         allow-query{any;};
	        
	         recursion yes;
	         dnssec-enable no;
	         dnssec-validation no;
};

Zone “.” IN {
		type hint;
		file “/var/named/named.ca”;
};

Step3. We save our root server detail in stld server.
#vi /var/named/etc/named.ca
; <<>> DiG 9.9.4-RedHat-9.9.4-38.el7_3.2 <<>> +bufsize=1200 +norec @a.root-servers.net
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 17380
;; flags: qr aa; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 27

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1472
;; QUESTION SECTION:
;.                          	IN  	NS

;; ANSWER SECTION:
.                   	518400  IN  	NS  	s.root-servers.net.

;; ADDITIONAL SECTION:
s.root-servers.net. 	3600000 IN  	A   	192.168.1.12

;; Query time: 18 msec
;; SERVER: 198.41.0.4#53(198.41.0.4)
;; WHEN: Po kvÄEST 2017
;; MSG SIZE  rcvd: 811

Step4. Start the service.
# systemctl start named

2.7.5. Checking Forward DNS Hierarchy 

Now we come on client which Ip is 192.168.1.90 and we give the Recursive Resolver IP which is 192.168.1.15.and check forward DNS server by command.

#nslookup (for checking query about our domain name)

>server 192.168.1.15 (set the recursive reolver server ip addres)

>set type=ns (it is used for name address resource record)

>. (it tells about root server name )

>in.(it tells about root server name )

>cdac.in. (it tells about stld server name)

>set type=A (it is used for ipv4 address resource record)
 
>. (it tells about root server ipv4 address)

>in. (it tells about tld server ipv4 address)
>www.cdac.in (it tells about ip address of domain name)
2.8 Implementation of Reverse DNS Hierarchy Test-Bed

In this Reverse DNS hierarchy we installed and configured DNS Test Bed Hierarchy by configuring Root, TLD, STLD, TTLD DNS Servers for Reverse zone. 

2.8.1 Root Server Configuration
First we configure root server on virtual machine in 192.168.1.12 and host name is s.root-server.net.  It has 8 Gb memory. Now we configure root server stepwise. 

Step 1. Install the packages of DNS.
# yum install bind bind-utils
Step 2.Create the zone file for “.” in configuration file of bind.
# vi /etc/named.conf
      Options{
                    listen -on port 53 {192.168.1.12;};
	//       listen -on -v6 port 53 {::!;};
	         Allow-query{any;};
	        
	         recursion no;
	         dnssec-enable no;
	         dnssec-validation no;
};

Zone “.” IN {
		type master;
		File “/var/named/root-zone”;
};

Step3. Create root-zone file which can stored the resources record and which can be specify the domain name IP address for next level which is called tld.

        $TTL 1D
            @   	IN SOA  s.root-servers.net. 	root.s.root-servers.net.(
                                    	                                   2   	; serial
                                    	                                  1D  	; refresh
                                    	                                   1H  	; retry
                                    	                                   1W  	; expire
                                    	                                    3H  	; minimum
             );
             @   	IN  	NS  	s.root-servers.net.
              s.root-servers.net. 	IN  	A   	192.168.1.12
              in. 	NS  	ns.tld.in.
              arpa.   NS  	ns.tld.in.
              ns.tld.in.  	A   	192.168.1.13
	Step4. Start the service.
# systemctl start named

2.8.2 Top Level Domain Configuration
Now we configure tld server on virtual machine in 192.168.1.13 and host name is ns.tld.in . It has 8 Gb memory. Now we configure tld server stepwise. 
Step 1. Install the packages of DNS.
# yum install bind bind-utils

Step 2.Create the zone file for “in” in configuration file of bind.
# vi /etc/named.conf

      Options{
                    listen -on port 53 {192.168.1.13;};
	//       listen -on -v6 port 53 {::!;};
	         Allow-query{any;};
	        
	         recursion no;
	         dnssec-enable no;
	         dnssec-validation no;
       };

Zone “.” IN {
		type hint;
		file “/var/named/named.ca”;
};
zone "arpa" {
    	type master;
    	file "zone-arpa";
};

Step3. We save our root server detail in tld server.
#vi /var/named/etc/named.ca
; <<>> DiG 9.9.4-RedHat-9.9.4-38.el7_3.2 <<>> +bufsize=1200 +norec @a.root-servers.net
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 17380
;; flags: qr aa; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 27

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1472
;; QUESTION SECTION:
;.                          	IN  	NS

;; ANSWER SECTION:
.                   	518400  IN  	NS  	s.root-servers.net.

;; ADDITIONAL SECTION:
s.root-servers.net. 	3600000 IN  	A   	192.168.1.12

;; Query time: 18 msec
;; SERVER: 198.41.0.4#53(198.41.0.4)
;; WHEN: Po kvÄEST 2017
;; MSG SIZE  rcvd: 811

step4.Create root-in file which can stored the resources record and which can be specify the domain name IP address for next level which is called second top level Domain (STLD).

#vi /var/named/zone-arpa
$TTL 1D;
@   	IN  	SOA 	ns.tld.in.  	root.ns.tld.in. (
                                    	2;
                                    	1D;
                                    	1H;
                                    	1W;
                                    	3H;
);
@   	IN  	NS  	ns.tld.in.
ns.tld.in.  	A   	192.168.1.13
in-addr.arpa.   NS  	ns.cdac.in.
ns.cdac.in. 	A   	192.168.1.14

Step5. Start the service.
# systemctl start named

2.8.3 Second Top Level Domain Configuration 
Now we configure stld server on virtual machine in 192.168.1.14 and host name     
is ns.cdac.in. It has 8 Gb memory. Now we configure stld server stepwise.  

Step 1. Install the packages of DNS.
# yum install bind bind-utils

Step 2.Create the zone file for “cdac.in” in configuration file of bind.
# vi /etc/named.conf

      Options{
                    listen -on port 53 {192.168.1.14;};
	//       listen -on -v6 port 53 {::!;};
	         Allow-query{any;}

	         recursion no;
	         dnssec-enable no;
	         dnssec-validation no;
};

Zone “.” IN {
		type hint;
		file “/var/named/named.ca”;
};
zone "cdac.in" IN {
    	type master;
    	file "zone-cdac";
};
zone "in-addr.arpa" IN {
    	type master;
    	file "zone-addr";
};

Step3. We save our root server detail in stld server.
#vi /var/named/etc/named.ca
; <<>> DiG 9.9.4-RedHat-9.9.4-38.el7_3.2 <<>> +bufsize=1200 +norec @a.root-servers.net
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 17380
;; flags: qr aa; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 27

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1472
;; QUESTION SECTION:
;.                          	IN  	NS

;; ANSWER SECTION:
.                   	518400  IN  	NS  	s.root-servers.net.

;; ADDITIONAL SECTION:
s.root-servers.net. 	3600000 IN  	A   	192.168.1.12

;; Query time: 18 msec
;; SERVER: 198.41.0.4#53(198.41.0.4)
;; WHEN: Po kvÄEST 2017
;; MSG SIZE  rcvd: 811


step4.Create zone-cdac file which can stored the resources record and which can be specify the domain name IP address for next level which is called authoriative server.

	#vi /var/named/zone-addr
	$TTL 1D;
@   	IN  	SOA 	ns.cdac.in. 	root.ns.cdac.in. (
                                    		4;
                                    		1D;
                                    		1H;
                                    		1W;
                                    		3H;	
);
@   	IN  	NS  	ns.cdac.in.
ns.cdac.in. 	A   	192.168.1.14
152.139.14.in-addr.arpa.    	NS  	ns1.ns.cdac.in.

	Step5. Now we give the name server of  third top level domain of 
authoritative server of reverse hierarchy in forward  hierarchy authoritative server.
$TTL 1D
@   	IN  	SOA 	ns.cdac.in. 	root.ns.cdac.in. (
                                    	3   	; serial
                                    	1D  	; refresh
                                    	1H  	; retry
                                    	1W  	; expire
                                    	3H  	; minimum

);
@   	IN  	NS  	ns.cdac.in.
ns          	A   	192.168.1.14
www         	A   	192.168.1.14
ns1.ns.cdac.in. A   	192.168.1.16

Step6. Start the service.
# systemctl start named



2.8.4. Third Top Level  Domain Configuration Or Authoritative Server

Now we configure ttld server on virtual machine in 192.168.1.16 and host name is ns1.ns.cdac.in . It has 8 Gb memory. Now we configure tld server stepwise.

Step 1. Install the packages of DNS.
# yum install bind bind-utils

Step 2.Create the zone file for “cdac.in” in configuration file of bind.
# vi /etc/named.conf

      Options{
                    listen -on port 53 {192.168.1.14;};
	//       listen -on -v6 port 53 {::!;};
	         Allow-query{any;}

	         recursion no;
	         dnssec-enable no;
	         dnssec-validation no;
};

Zone “.” IN {
		type hint;
		file “/var/named/named.ca”;
};
zone "152.139.14.in-addr.arpa" IN {
    	type master;
    	file "zone-reverse";
};

Step3. We save our root server detail in ttld server.
#vi /var/named/etc/named.ca
; <<>> DiG 9.9.4-RedHat-9.9.4-38.el7_3.2 <<>> +bufsize=1200 +norec @a.root-servers.net
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 17380
;; flags: qr aa; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 27

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1472
;; QUESTION SECTION:
;.                          	IN  	NS

;; ANSWER SECTION:
.                   	518400  IN  	NS  	s.root-servers.net.

;; ADDITIONAL SECTION:
s.root-servers.net. 	3600000 IN  	A   	192.168.1.12

;; Query time: 18 msec
;; SERVER: 198.41.0.4#53(198.41.0.4)
;; WHEN: Po kvÄEST 2017
;; MSG SIZE  rcvd: 811

step4.Create zone-cdac file which can stored the resources record and which can be specify the domain name IP address for next level which is called authoriative server.
#vi /var/named/zone-reverse

$TTL 1D
$ORIGIN 152.139.14.in-addr.arpa.
@   	IN  	SOA 	ns1.ns.cdac.in. root.ns1.ns.cdac.in. (
                                    	4   	; serial
                                    	1D  	; refresh
                                    	1H  	; retry
                                    	1W  	; expire
                                    	3H  	; minimum

);
@   	IN  	NS  	ns1.ns.cdac.in.
ns1.ns.cdac.in. A   	192.168.1.16
56  	PTR 	www.cdac.in.

Step5. Start the service.
# systemctl start named

2.8.5. Recursive Resolver Configuration
Now we configure Recursive Resolver server on virtual machine in 192.168.1.13 and host name is recursive. It has 8 Gb memory. Now we configure resolver server stepwise.  

Step 1. Install the packages of DNS.
# yum install bind bind-utils

Step 2.Create the changes for Recursive resolver  in configuration file of bind.
# vi /etc/named.conf

      Options{
 listen -on port 53 {192.168.1.15;};
	//       listen -on -v6 port 53 {::!;};
	         allow-query{any;};
	        
	         recursion yes;
	         dnssec-enable no;
	         dnssec-validation no;
};

Zone “.” IN {
		type hint;
		file “/var/named/named.ca”;
};

Step3. We save our root server detail in stld server.
#vi /var/named/etc/named.ca
; <<>> DiG 9.9.4-RedHat-9.9.4-38.el7_3.2 <<>> +bufsize=1200 +norec @a.root-servers.net
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 17380
;; flags: qr aa; QUERY: 1, ANSWER: 13, AUTHORITY: 0, ADDITIONAL: 27

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1472
;; QUESTION SECTION:
;.                          	IN  	NS

;; ANSWER SECTION:
.                   	518400  IN  	NS  	s.root-servers.net.

;; ADDITIONAL SECTION:
s.root-servers.net. 	3600000 IN  	A   	192.168.1.12

;; Query time: 18 msec
;; SERVER: 198.41.0.4#53(198.41.0.4)
;; WHEN: Po kvÄEST 2017
;; MSG SIZE  rcvd: 811

Step4. Start the service.
# systemctl start named

2.8.6. Checking Reverse DNS Hierarchy 
Now we come on client which Ip is 192.168.1.90 and we give the Recursive Resolver IP which is 192.168.1.15.and check reverse DNS server by command.

#nslookup (for checking query about our domain name)

>server 192.168.1.15 (set the recursive reolver server ip addres)

>set type=ns (it is used for name address resource record)

>. (it tells about root server name )

>arpa.(it tells about root server name )

>in-addr.arpa. (it tells about stld server name)

>152.139.14.in-addr.arpa. (it tells about ttld server name)

>set type=PTR (it is used for domain name  resource record)

>14.139.152.56 (it tells about domain name) 


3 DOMAIN NAME SYSTEM SECURITY EXTENSION

The Domain Name System Security Extensions (DNSSEC)is an Internet service that enhances DNS with a few security features. It provides DNS clients and resolvers with origin authentication of DNS data, authenticated, data integrity, but not availability or confidentiality.
All these benefits that DNSsec provides, comes from the signing of the information that DNSAs a result, a DNS resolver that receives the answer, knows the authenticated origin of the DNS information and can verify it by contacting the authoritative nameserver and evaluate the integrity of its data. In this way, it also authenticates who signed the data, provides non-repudiation and protection of the resolvers from forged or manipulated DNS data created by cache poisoning. Moreover, DNSsec provides a secure bonding between child and parent zones (chain of trust). By doing this, it protects nameservers from possible attacks and manipulation of their data or use of fake nameservers. As a result, the parent zone can verify the integrity of child zone’s data and notify the user about it.
DNSsec does not provide protection against (D)DoS attacks and manipulation of the data as they travel through the wire because DNS data are public and they travel in clear text resulting in possible spoofing. Other technologies or mechanisms can protect from these effects such as IPSec, SIG  but not in all aspects of DNS and without increasing its complexity.

3.1 Key Used In DNSSEC

3.1.1 Zone Signing Keys
Each zone in DNSSEC has a zone-signing key pair (ZSK): the private portion of the key digitally signs each RRset in the zone, while the public portion verifies the signature. To enable DNSSEC, a zone operator creates digital signatures for each RRset using the private ZSK and stores them in their name server as RRSIG records. 

However, these RRSIG records are useless unless DNS resolvers have a way of verifying the signatures. The zone operator also needs to make their public ZSK available by adding it to their name server in a DNSKEY record.
When a DNSSEC resolver requests a particular record type (e.g., AAAA), the name server also returns the corresponding RRSIG. The resolver can then pull the DNSKEY record containing the public ZSK from the name server. Together, the RRset, RRSIG, and public ZSK can validate the response. If we trust the zone-signing key in the DNSKEY record, we can trust all the records in the zone. But, what if the the zone-signing key was compromised? We need a way to validate 
the public ZSK.

3.1.2 Key-Signing Key
In addition to a zone-signing key, DNSSEC name servers also have a key-signing key (KSK). The KSK validates the DNSKEY record in exactly the same way as our ZSK secured the rest of our RRsets in the previous section: It signs the public ZSK (which is stored in a DNSKEY record), creating an RRSIG for the DNSKEY. Just like the public ZSK, the name server publishes the public KSK in another DNSKEY record, which gives us the DNSKEY RRset shown above. Both the public KSK and public ZSK are signed by the private KSK. Resolvers can then use the public KSK to validate the public ZSK. We separate zone-signing keys and key-signing keys because it’s difficult to replace an old or compromised KSK instead changing the ZSK is much easier. 
We’ve now established trust within our zone, but DNS is a hierarchical system, and zones rarely operate independently. Complicating things further, the key-signing key is signed by itself, which doesn’t provide any additional trust. We need a way to connect the trust in our zone with its parent zone.

3.2 DNSSEC Resource Record
DNSsec introduces some new attributes and records needed in order to provide the desired security. It offers public-key cryptography and applies it in the records of a zone by digitally signing them. The correct DNSKEY record is authenticated via a chain of trust, starting with a set of verified public keys for the DNS root zone which is the trusted third party and ending with the domain zone. The proper authentication is possible only if the domain administrator has uploaded the hash of the DNSKEY (KSK) to the parent zone.

3.2.1 RRsets
The first step towards securing a zone with DNSSEC is to group all the records with the same type into a resource record set (RRset). For example, if you have three AAAA records in your zone, they would all be bundled into a single AAAA RRset and it's the whole RRset that will be digitally signed.
 
                                                Fig.3.2.1 RRsets
                                                
3.2.2 RRSIG – Contains a cryptographic signature the ZSK private key is used to generate a digital signature, known as a Resource Record Signature (RRSIG), for each of the resource record sets (RRSET) in a zone. The ZSK public key is stored in the DNS to authenticate an RRSIG. Each name within a DNSSEC signed zone will be covered by an RRSIG.
                                                Fig.3.2.2 RRSIG
                                                
3.2.3 DNSKEY –DNSKEY is use to store the public key of zone signing key and key signing key.

 
                                                 Fig.3.2.3 DNSkey

3.2.4 Delegation Signer – DNSSEC introduces a delegation signer (DS) record to allow the transfer of trust from a parent zone to a child zone. A zone operator hashes the DNSKEY record containing the public KSK and gives it to the parent zone to publish as a DS record. Every time a resolver is referred to a child zone, the parent zone also provides a DS record. This DS record is how resolvers know that the child zone is NSEC-enabled. To check the validity of the child zone’s public KSK, the resolver hashes it and compares it to the DS record from the parent. If they match, the resolver can assume that the public KSK hasn’t been tampered with, which means it can trust all of the records in the child zone. This is how a chain of trust is established in DNSSEC. Note that any change in the KSK also requires a change in the parent zone’s DS record. Changing the DS record is a multi-step process that can end up breaking the zone if it’s performed incorrectly. First, the parent needs to add the new DS record, then they need to wait until the TTL for the original DS record to expire before removing it. This is why it’s much easier to swap out zone-signing keys than key-signing keys.

 
                                                Fig.3.2.4 Delegation Signer(DS)



3.3 The Chain of Trust

DNSSEC has a hierarchical trust model. To securely resolve a name in DNSSEC, a root public key must be available at the resolver. Public keys themselves are authenticated either by being statically configured into a DNS server or resolver, or by being signed by a parent zone’s private key. The chain of trust works by following secured delegation pointers called Delegation Signers (DSs). The DS record delegates trust from a parental key to a child’s zone key. The DS record holds a SHA-1 hash of a child’s zone public key, and this hash is signed with the zone private key of the parent. By checking the signature of the DS record, a resolver can validate the hash of the child’s zone public key, and can in turn establish the validity of the child’s public key. A root public key injection attack at the client end, as described in, would compromise this chain of trust. Any compromise in any of the zones between the root and a particular target name will result in data being marked as bogus, which may cause entire sub-domains to become invisible to verifying clients. Data published on an authoritative primary server will not be seen immediately by verifying clients; it may take some time for the data to be transferred to other secondary authoritative name-servers. During this period, clients may be fetching data from caching non-authoritative servers. For the verifying clients it is im-
portant that data from secured zones can be used to build chains of trust regardless of whether the data came directly from an authoritative server or a caching name-server.
 
 

                                                              Fig.3.3 Chain of Trust

 


3.4 Implementation of DNSSEC Hierarchy testbed

We are generating Public and Private keys for signing Resource Record and zone file in DNS Test Bed Hierarchy by configuring Root, TLD, STLD DNS Servers for Hierarchy. 



3.4.1 Configuration Of DNSSEC IN Root Server
Apply  DNSSEC on the root server whose IP address is 192.168.1.12 and by these command we are apply DNSSEC.

Step 1: Make directory for keys
# mkdir keys

Step 2: Go to keys file
# cd keys

Step 3: Generate Public and Private ZSK for . zone
# dnssec-keygen -a RSASHA256 -b 1024 .
In that command -a means algorithm which is RSASHA256 and -b means bits of keys.we did 1024 bit keys for ZSK.

Step 4: Generate Public and Private KSK for . zone
#dnssec-keygen -a RSASHA256 -b 2048 -f KSK .
We did 2048 bit keys for KSK.

Step 5: open the zone file and include the public key in last of zone file.
# vi /var/named/root-zone
$ include /var/named/keys/forward/K.+005 + 16324.key
$ include /var/named/keys/forward/K.+005 + 65013.key

Step 6: Now we sign the zone file.
# dnssec -signzone -o . ../root-zone 

Step 7: open the configuration  file and give the path of signed file and enable the DNSSEC.
# vi /etc/named.conf
      Options{
                    listen -on port 53 {192.168.1.12;};
	//       listen -on -v6 port 53 {::!;};
	         Allow-query{any;};
	        
	         recursion no;
	         dnssec-enable yes;
	         dnssec-validation yes;
};

Zone “.” IN {
		type master;
		File “/var/named/root-zone.signed”;
};

Step 8: Start the service.
# systemctl start named


3.4.2 Configuration Of DNSSEC In TLD Server
Apply  DNSSEC on the tld server whose IP address is 192.168.1.13 and by these command we are apply DNSSEC.

Step 1: Make directory for keys
# mkdir keys

Step 2: Make the directory for forward  zone keys in keys folder
#mkdir forward

Step 3: Generate Public and Private ZSK for “in” zone which is for forward zone file.
# dnssec-keygen -a RSASHA256 -b 1024 .
In that command -a means algorithm which is RSASHA256 and -b means bits of keys.we did 1024 bit keys for ZSK.

Step 4: Generate Public and Private KSK for “in” zone which is for forward zone file.
#dnssec-keygen -a RSASHA256 -b 2048 -f KSK .
We did 2048 bit keys for KSK.

Step 5: Make the directory for reverse keys in keys folder
#mkdir reverse

Step 6: Generate Public and Private ZSK for “arpa” zone which is for reverse zone file.
# dnssec-keygen -a RSASHA256 -b 1024 .
In that command -a means algorithm which is RSASHA256 and -b means bits of keys.we did 1024 bit keys for ZSK.

Step 7: Generate Public and Private KSK for “arpa” zone which is for reverse zone file.
#dnssec-keygen -a RSASHA256 -b 2048 -f KSK .
We did 2048 bit keys for KSK.

Step 8: open the forward zone file and include the public key of “in” zone  in last of zone file.
# vi /var/named/zone-in
$ include /var/named/keys/forward/K.+005 + 26324.key
$ include /var/named/keys/forward/K.+005 + 75013.key

Step 9: Now we sign the forward zone file.
# dnssec -signzone -o . ../zone-in 

Step 10: open the reverse zone file and include the public key of “arpa” zone  in last of zone file.
# vi /var/named/zone-arpa
$ include /var/named/keys/forward/K.+005 + 36324.key
$ include /var/named/keys/forward/K.+005 + 85013.key

Step 11: Now we sign the reverse zone file.
# dnssec -signzone -o . ../zone-arpa 

Step 12: open the configuration  file and give the path of reverse and forward signed file in zone and enable the DNSSEC.
# vi /etc/named.conf
      Options{
                    listen -on port 53 {192.168.1.12;};
	//       listen -on -v6 port 53 {::!;};
	         Allow-query{any;};
	        
	         recursion no;
	         dnssec-enable yes;
	         dnssec-validation yes;
};
zone "." IN {
    	type hint;
    	file "named.ca";
};

zone "in" IN {
    	type master;
    	file "zone-in.signed";
};
zone "arpa" {
    	type master;
    	file "zone-arpa.signed";
};

Step 13:Give the DS record of “in” zone and “arpa” zone in root-zone signed file in root server at last.
in.                 	IN DS 32566 8 1 2D9D13F2A082EB57B5A4F8B2CDBF9264AC1EAF5E

in.                 	IN DS 32566 8 2 F0904F5BC08A4663CD991633E182DC4FB02DD63D541DC33E3F749987 385CB940

arpa.               	IN DS 49669 8 1 7D27553A9FF4ABC3120BDA5EB03834E3DC6AAF3C

arpa.               	IN DS 49669 8 2 55CDD89BED648A47084FF3D885F1AC8CB5359D0497591511739F55A4 5CAA1F28

Step 14: Start the service.
# systemctl start named


3.4.3 CONFIGURATION OF DNSSEC IN STLD SERVER
Apply  DNSSEC on the tld server whose IP address is 192.168.1.14 and by these command we are apply DNSSEC.

Step 1: Make directory for keys
# mkdir keys

Step 2: Make the directory for forward  zone keys in keys folder
#mkdir forward

Step 3: Generate Public and Private ZSK for “cdac.in” zone which is for forward zone file.
# dnssec-keygen -a RSASHA256 -b 1024 .
In that command -a means algorithm which is RSASHA256 and -b means bits of keys.we did 1024 bit keys for ZSK.

Step 4: Generate Public and Private KSK for “cdac.in” zone which is for forward zone file.
#dnssec-keygen -a RSASHA256 -b 2048 -f KSK .
We did 2048 bit keys for KSK.

Step 5: Make the directory for reverse keys in keys folder
#mkdir reverse

Step 6: Generate Public and Private ZSK for “in-addr.arpa” zone which is for reverse zone file.
# dnssec-keygen -a RSASHA256 -b 1024 .
In that command -a means algorithm which is RSASHA256 and -b means bits of keys.we did 1024 bit keys for ZSK.

Step 7: Generate Public and Private KSK for “in-addr.arpa” zone which is for reverse zone file.
#dnssec-keygen -a RSASHA256 -b 2048 -f KSK .
We did 2048 bit keys for KSK.

Step 8: open the forward zone file and include the public key of “cdac.in” zone  in last of zone file.
# vi /var/named/zone-cdac
$ include /var/named/keys/forward/K.+005 + 46324.key
$ include /var/named/keys/forward/K.+005 + 95013.key

Step 9: Now we sign the forward zone file.
# dnssec -signzone -o . ../zone-cdac 

Step 10: open the reverse zone file and include the public key of “in-addr.arpa” zone  in last of zone file.
# vi /var/named/zone-arpa
$ include /var/named/keys/forward/K.+005 + 56324.key
$ include /var/named/keys/forward/K.+005 + 65013.key

Step 11: Now we sign the reverse zone file.
# dnssec -signzone -o . ../zone-arpa 

Step 12: open the configuration  file and give the path of reverse and forward signed file in zone and enable the DNSSEC.
# vi /etc/named.conf
      Options{
                    listen -on port 53 {192.168.1.12;};
	//       listen -on -v6 port 53 {::!;};
	         Allow-query{any;};
	        
	         recursion no;
	         dnssec-enable yes;
	         dnssec-validation yes;
};
zone "." IN {
    	type hint;
    	file "named.ca";
};

zone "cdac.in" IN {
    	type master;
    	file "zone-in.signed";
};
zone "in-addr.arpa" {
    	type master;
    	file "zone-arpa.signed";
};

Step 13:Give the forward zone  DS record “cdac.in” in the forward zone signed file in tld server.
cdac.in.            	IN DS 38050 8 1 828E2575cdac314A9BFEC2990B7E2E2BA2769DB3F1A6

cdac.in.            	IN DS 38050 8 2 78F448DF07F5655C1A6CC4DA92939110CD78705DAD97E692201DA323 DB29CF32

Step 14:Give the reverse zone  DS record “in-addr.arpa” in the forward zone signed file in tld server.
in-addr.arpa.       	IN DS 53926 8 1 C7FA7AEF2F79419BF5A1E0EC3BE7F509FFDCEE12

in-addr.arpa.       	IN DS 53926 8 2 18BF85F1F3E57B8346CF852ED01B91AB813064785DCEB3CD08F27E44 9AADF47B

Step 15: Start the service.
# systemctl start named
	


3.4.4 Configuration Of DNSSEC In ttld Server

Apply  DNSSEC on the ttld server whose IP address is 192.168.1.16 and by these command we are apply DNSSEC.

Step 1: Make directory for keys
# mkdir keys

Step 2: Go to keys file
# cd keys

Step 3: Generate Public and Private ZSK for “152.139.14.in-addr.arpa” zone
# dnssec-keygen -a RSASHA256 -b 1024 .
In that command -a means algorithm which is RSASHA256 and -b means bits of keys.we did 1024 bit keys for ZSK.

Step 4: Generate Public and Private KSK for “152.139.14.in-addr.arpa” zone
#dnssec-keygen -a RSASHA256 -b 2048 -f KSK .
We did 2048 bit keys for KSK.

Step 5: open the zone file and include the public key in last of zone file.
# vi /var/named/root-reverse
$ include /var/named/keys/forward/K.+005 + 86324.key
$ include /var/named/keys/forward/K.+005 + 99013.key

Step 6: Now we sign the zone file.
# dnssec -signzone -o . ../root-zone 

Step 7: open the configuration  file and give the path of signed file and enable the DNSSEC.
# vi /etc/named.conf
      Options{
                    listen -on port 53 {192.168.1.12;};
	//       listen -on -v6 port 53 {::!;};
	         Allow-query{any;};
	        
	         recursion no;
	         dnssec-enable yes;
	         dnssec-validation yes;
};
zone "152.139.14.in-addr.arpa" IN {
    	type master;
    	file "zone-reverse.signed";
};

Step 8:Give the reverse zone  DS record “152.139.14.in-addr.arpa” in the reverse zone signed file in stld server.
152.139.14.in-addr.arpa. IN DS 63667 8 1 664016EB9D8CFDB4D228C7B9564D05885F8CD566

152.139.14.in-addr.arpa. IN DS 63667 8 2 26E57EB73E2A838E9E5949E69037AC1A401E79F124AF8D282B2B77F0 A4B2C023

Step 9: Start the service.
# systemctl start named


3.4.5 Configuration Of DNSSEC In Recursive Server
Step 1: open the configuration  file and enable the DNSSEC.
# vi /etc/named.conf
      Options{
                    listen -on port 53 {192.168.1.12;};
	//       listen -on -v6 port 53 {::!;};
	         Allow-query{any;};
	        
	         recursion no;
	         dnssec-enable yes;
	         dnssec-validation yes;
};

Step 2:copy the root server public key and paste in the root key of recursive server.
#vi /etc/named.root.key
managed-keys {
    	. initial-key 256 3 8 "AwEAAeUtVr/5G8zeQRqEV578knnYwn0mt/LgCZ2M3uuk5tbG+J4Zz2L4 pRqOk9kTbJnHJedr5aWqVeUiCB51jVQfU02xbPv3gcI9jsQNWGhK13bH teVL+TJWyuzaCjZXWJAA3eq9e5PrI0nXgSEuI16LApMZVnwatMDQdnWv lZe3SEPH";
};

Step 3: Start the service.
# systemctl start named

3.5 LATENCY CHECKING BETWEEN DNS AND DNSSEC

3.5.1  Latency check between DNS and DNSSEC for Forward server

In this graph we take scale 1 box=1 msec

 
					Fig.  3.4






3.5.2 Latency check between DNS and DNSSEC for Reverse server server

In this graph we take scale 1 box=1 msec

 
					Fig.  3.5


3.6 Analysing the Packet Flow in tcpdump
When We query for www.cdac.in in recursive server we capture by tcpdump.

 
Fig.  3.6

3.7 DNS Header
 
			Fig.3.7.1 DNS Header
The header of the packet contains identifying information, along with hints (summaries) about what the rest of the message contains. The header is made up of six fields, sixteen bits each, for a total of twelve bytes. The first sixteen bits are for the Transaction ID, used to match the response to the query, and is created by the client on the query message and returned by the server in the response.

The next field is for flags. This is probably the most important part of the DNS packet, as these flags distinguish response from query, and iterative from recursive query. They are listed, in order:
●	Bit 1: QR, query/response flag. When 0, message is a query. When 1, message is response.
●	Bits 2-5: Opcode, operation code. Tells receiving machine the intent of the message. Generally 0 meaning normal query, however there are other valid options such as 1 for reverse query and 2 for server status.
●	Bit 6: AA, authoritative answer. Set only when the responding machine is the authoritative name server of the queried domain.
●	Bit 7: TC, truncated. Set if packet is larger than the UDP maximum size of 512 bytes.
●	Bit 8: RD, recursion desired. If 0, the query is an iterative query. If 1, the query is recursive.
●	Bit 9: RA, recursion available. Set on response if the server supports recursion.
●	Bit 10: Z. Reserved for future use, must be set to 0 on all queries and responses.
●	Bit 11: AD, authentic data. Used in DNSSEC. Considered part of Z in older machines.
●	Bit 12: CD, checking disabled. Used in DNSSEC. Considered part of Z in older machines.
●	Bit 13-16: Rcode, return code. It will generally be 0 for no error, or 3 if the name does not exist.
Question
The question is present in both the query and the response, and should be identical. Some tools, like Wireshark, call it “Query” like A IPv4 address,  AAAA -Quad IPv6 address, NS- Name server record, MS-mail exchange record.
 Answer Resource Records
The answer consists of the resource records that answer the question. The “Answer” section is present on the response from recursive resolver to an end user’s computer, or in the response from the authoritative name server of the domain to the recursive resolver. It depends on the type of question, but for DNS resolution of web domains it will be A, AAAA, or CNAME. CNAME stands for Canonical Name, it means that the domain in the query is an alias for another domain.
The data field contents vary depending on the type of record, but here are the major ones:

●	A: One data field storing a four-byte IPv4 address

●	AAAA: One data field storing a sixteen-byte IPv6 address.

Authority Resource Records
When a name server like TLDs does not have the answer to the query (as is not authoritative), it will not send answer records. Instead it will populate the authority section with all of the name servers that it knows as authoritative to the domain or part of the domain tree (like .com), if it has them. These NS records are different from A records as they have a domain name in both the RR name and RR data fields. Unlike the answer section, the authority section can only have NS records. Note that NS records can be sent in other sections.

Additional Records
Additional or “glue” records help avoid additional recursion by providing the IP addresses (A or AAAA records) of the NS records sent in the authoritative or answer section, so that those domains do not need to be resolved. These RRs are usually the A and/or AAAA records for the NS records in the authority section, although they can be any record.

4.  CONCLUSION :
In this project we  installed and configured DNS Test Bed Hierarchy by configuring Root, TLD, STLD DNS Servers for forward and reverse lookups zones. We done the evaluation on this hierarchy to check DNS query latency from the identified vantage point. We implemented DNSSEC on DNS Test Bed Hierarchy and again done the evaluation for DNS query latency, and done the analysis to compare the query latency with DNSSEC and without DNSSEC.







5.  REFERENCE :
 
https://goo.gl/nsUFwy
https://www.researchgate.net/publication/326894081_Domain_Name_System_DNS_Security_Attacks_Identification_and_Protection_Methods
https://ieeexplore.ieee.org/document/6682730
 
https://en.wikipedia.org/wiki/Domain_Name_System_Security_Extensions
 
https://medium.com/iocscan/how-dnssec-works-9c652257be0
 	https://www.cloudflare.com/dns/dnssec/how-dnssec-works/
 
https://www.digitalocean.com/community/tutorials/how-to-setup-dnssec-on-an-authoritative-bind-dns-server--2
 	https://www.youtube.com/watch?v=RlPh6cDesxg
 
https://www.youtube.com/watch?v=_8M_vuFcdZU&t=587s

















        
