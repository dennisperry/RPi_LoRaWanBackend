# RPi_LoRaWanBackend
Intallation and configuration instructions for LoRa Micro Gateway using Semtech packet_forwarder and Chirpstack Gateway-Bridge, Network-Server and Application-Server.

**General Overview**
The major components to make the LoRaWan backend include; the Semtech packet forwarder, the Chirpstack-gateway-bridge, the Chirpstack-network-server and the Chirpstack-application-server.

**Major Components**
Semtech packet_forwarder
1. Build the Lora-net/lora_gateway library. Code here: https://github.com/Lora-net/lora_gateway . This library is needed to build the next item.
2. Build the Lora-net/packet_forwarder. Code here: https://github.com/Lora-net/packet_forwarder . This code will need several modifications. First; you will likely want to put the configuration files in /etc/packet_forwarder/. To do this you will need to edit the location the packet_forwarder looks for configureation files. The line that set the location are found at the beginning of main().
Second; you will likely want to add in a reset of your RAK concentrator. My RAK831 used BCM GPIO17, expansion connector pin 11, wiringPi GPIO0. I used the wiringPi library to make my changes.
Third; you will need to specify the path to the tty port your GPS is connected. This can be found around line 162. There is also likely a syntax for putting this in the global_conf.json but I've not seen an example of this.
3. The packet forwarder will pass its packets to an MQTT server. Mosquitto can be installed for this purpose.
4. Once you know your lora_pkt_fwd binary is working it can be placed in /usr/bin and then systemctl can be used to start it automatically at boot.
5. This repository will contain my configuration files as examples. The files have permissions set to 777 so that they could be copied to github but should be changed back to 640 once installed.
6. 'journalctl -u lora_pkt_fwd -f -n 100' can be used to examine the packet forwarder spew once it is under systemctl.

**Chirpstack-gateway-bridge**
1. Consult the installation instructions at https://www.chirpstack.io.
2. Go to the 'Requirements' section first which will give any necessary additional packages to install.
3. Follow the 'Debian/Ubuntu' instructions.
4. This repository will contain my configuration file. It will go in /etc/chirpstack-gateway-bridge/.
5. 'journalctl -u chirpstack-gateway-bridge -f -n 100' can be used to examine the gateway-bridge spew.

**Chirpstack-network-server**
1. Consult the installation instructions at https://www.chirpstack.io.
2. Again go to the 'Requirements' section first.
3. Follow the 'Debian/Ubuntu' instructions.
4. Use my configuration file to start. In /etc/chirpstack-network-server/.
5. 'journalctl -u chirpstack-network-server -f -n 100' can be used to examine the network-server spew.

**Chirpstack-application-server**
Instruction, requirements, Debian/Ubuntu, config file, journalctl.
My chirpstack-application-server.toml file sets the IP address of my machine and should be changed to match your network.

**Web Interface**
The chirpstack-application-server sets up a web interface for administering the whole system. By default this will on 'localhost:8080' but can be changed via the configuration file to use an external IP address resulting in the ability to administer the system from another machine and to run RPi headless.

**lora_pkt_fwd.c and Makefile**
Included are my modified lora_pkt_fwd.c and Makefile that changes the config file to /etc/packet_forwarder and resets the gateway using wiringPi.

**lora_pkt_fwd.service**
This file is placed in /etc/systemd/system/




