ZMap: The Internet Scanner
==========================

![Build Status](https://github.com/zmap/zmap/actions/workflows/cmake.yml/badge.svg)

ZMap is a fast single packet network scanner designed for Internet-wide network
surveys. On a typical desktop computer with a gigabit Ethernet connection, ZMap
is capable scanning the entire public IPv4 address space in under 45 minutes. With
a 10gigE connection and [PF_RING](http://www.ntop.org/products/packet-capture/pf_ring/),
ZMap can scan the IPv4 address space in under 5 minutes.

ZMap operates on GNU/Linux, Mac OS, and BSD. ZMap currently has fully implemented
probe modules for TCP SYN scans, ICMP, DNS queries, UPnP, BACNET, and can send a
large number of [UDP probes](https://github.com/zmap/zmap/blob/master/examples/udp-probes/README).
If you are looking to do more involved scans, e.g.,
banner grab or TLS handshake, take a look at [ZGrab 2](https://github.com/zmap/zgrab2),
ZMap's sister project that performs stateful application-layer handshakes.

Installation
------------

if you want to use zmap for rdns scanning, you have to install this version of zmap from source.

**Instructions on building ZMap from source** can be found in [INSTALL](INSTALL.md).

Usage
-----

create sql table:

    create table zmap_result( id int not null auto_increment PRIMARY KEY, 
                                ip varchar(20) not null, 
                                aa tinyint(1), ra tinyint(1), rcode tinyint(1), 
                                time timestamp not null DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP);

To scan recursive dns resolvers, you have to use the udp module and csv4rdns output module, 
the data field and saddr field must be specified in the output fields argument. Here is an example below.

    zmap -M udp -p 53 --probe-args=file:examples/udp-probes/dns_53_queryAwww.google.com.pkt -O csv4rdns -o result.csv --output-fields="saddr,data" 8.8.8.8
    
To save the outputs into mysql, use the following cmd:

    sudo zmap -M udp -p 53 --probe-args=file:examples/udp-probes/dns10.pkt -O sql -o localhost:root:root:rdns_scan:zmap_result --output-fields="saddr,data" --output-filter="repeat=0" 1.1.1.1
    
the -o argument is in the format of addr:user:pwd:database:table indicating the sql information instead of the filename.

License and Copyright
---------------------

ZMap Copyright 2017 Regents of the University of Michigan

Licensed under the Apache License, Version 2.0 (the "License"); you may not use
this file except in compliance with the License. You may obtain a copy of the
License at http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed
under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
CONDITIONS OF ANY KIND, either express or implied. See LICENSE for the specific
language governing permissions and limitations under the License.
