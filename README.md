# openair-cn-cups

## Development - First steps using Visual Studio Code

1. Download vscode available [here](https://code.visualstudio.com/download)
1. Clone the repository openair-cn-cups
1. Open the console in your workspace
1. Create a macvlan docker network  running ```make docker-create-network```.\
It is assumed that you have two network device, one of them is the enp0s20f0u1 (See [Makefile](Makefile)).
1. Setup [Developing inside a Container](https://code.visualstudio.com/docs/remote/container) in vscode
1. [Open the repository workspace using vscode in a conteiner](https://code.visualstudio.com/docs/remote/containers#_open-an-existing-workspace-in-a-container) (it will take some time)
1. Setup conteiner network: ```make docker-setup-network```  
1. Clone upf-bpf in `build/ext`: `git clone git@github.com:navarrothiago/upf-bpf.git build/ext`
1. Setup upf-bpf environment (install dependencies and creates veth pairs): `make setup`
1. Run spwgu standalone: `make run-spgwu`

## Environment

See [environment.](https://github.com/navarrothiago/masters/wiki/OpenAirInterface#lte-test-environment)

Pay attention with the MAC address and with the interface name that will be used.  
Because of there is a hardacoded definition, maybe you need to change the values in [Makefile](Makefile) of this project and in [Makefile](build/ext/upf-bpf/Makefile) of the upf-bpf project, or in scapy [script](build/ext/upf-bpf/tests/scripts/config_veth_pair.sh).

## Build code using Visual Studio Code

Follow the steps to build spgwu in vscode

1. Run again Build Task ```Ctrl + Shift + B```
1. Select ```Rebuild spgwu```
1. Check sucess message: **spgwu-test compiled** and **spgwu installed**

## Run UTs for spgwu

1. Run spgwu ```F5```

### Display all UDP sockets

Run the command ```ss -u -a```

Output:

```
State                   Recv-Q                    Send-Q                                        Local Address:Port                                        Peer Address:Port                   
UNCONN                  0                         0                                                127.0.0.11:48062                                            0.0.0.0:*                      
UNCONN                  0                         0                                             172.55.55.102:40280                                            0.0.0.0:*                      
UNCONN                  0                         0                                             172.55.55.102:8805                                             0.0.0.0:*                      
UNCONN                  0                         0                                              192.168.15.2:2152                                             0.0.0.0:* 
```

The following table maps the socket to the spgwu protocal stack:

app | socket
--- | --- 
spgwu-pfcp | 172.55.55.102:8805
spgwu-gtpv1_u | 192.168.5.2:2152

### IP Mapping

``` 
VM -> Docker
192.168.15.14 -> 192.168.15.2
10.50.11.227 -> 172.17.0.2
```
## Other commands

### Build spgwu docker image

```
make docker-build
```

### Build spgwu in debug mode

```
make debug
```

### Clean spgwu generated artifacts 

```
make clean
```

### Login spgwu docker container in bash mode

```
make shell-run
```



## Notes

```
Control User Plane Separation of SPGW-C and SPGW-U
├── http://www.openairinterface.org/?page_id=698 

It is distributed under OAI Public License V1.0. 
The license information is distributed under LICENSE file in the same directory.

The OpenAirInterface CN CUPS software is composed of the following parts: 

openair-cn-cups
├── build :       Build directory, contains targets and object files generated by compilation of network functions. 
    ├── log :     Directory containing build log files.
    ├── scripts : Directory containing scripts for building network functions
    ├── spgw_c :  Directory containing CMakefile.txt and object files generated by compilation of SPGW-C network function. 
    ├── spgw_u :  Directory containing CMakefile.txt and object files generated by compilation of SPGW-U network function. 
├── etc :         Directory containing the configuration files to be deployed for each network function.
└── scripts :     Directory containing scripts for the CI process
└── src :         Source files of network functions.
    ├── common :    Common header files
    │   ├── msg :   ITTI messages definitions.
    │   └── utils : Common utilities.
    ├── gtpv1u :    Generic GTPV1-U stack implementation
    ├── gtpv2c :    Generic GTPV2-C stack implementation
    ├── itti :      Inter task interface 
    ├── oai_spgwc : SPGW-C main directory, contains the "main" CMakeLists.txt file.
    ├── oai_spgwu : SPGW-U main directory, contains the "main" CMakeLists.txt file.
    ├── pfcp :      Generic PFCP stack implementation.
    ├── pgwc :      PGW-C network fonctions procedures and contexts.
    ├── sgwc :      SGW-C network fonctions procedures and contexts.
    ├── spgwu :     SPGW-U network fonctions procedures and contexts.
    │   └── simpleswitch : Very Basic Switch implementation.
    └── udp :       UDP server implementation.

RELEASE NOTES:

v1.0.0 -> First release, Able to serve a MME with basic attach, detach, release, paging procedures, default bearer only.
```
