
---

# miotyGO user guide

---

# ![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.001.png)

# ![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.002.png)

#

#

# Table of Contents

1. [Introduction](#introduction)
2. [Capabilities](#capabilities)
3. [Hardware](#hardware)
4. [Installation](#installation)
5. [Get shell access](#get-shell-access)
6. [Run the installer](#run-the-installer)
7. [Service Center configuration](#service-center-configuration)
8. [Register miotyGO on Service Center](#register-miotygo-on-service-center)
9. [Step 1: Create a new mioty network](#step-1-create-a-new-mioty-network)
10. [Step 2: Add a Base Station](#step-2-add-a-base-station)
11. [Step 3: Fill the Base Station EUI with miotyGO printed EUI](#step-3-fill-the-base-station-eui-with-miotygo-eui)
12. [Step 4: Fill the certificates](#step-4-fill-the-certificates)
13. [Attach a device](#attach-a-device)

|**Version**|**Comment**|**Date**|
| :- | :- | :- |
|v1.0|First version|16/02/24|
|v1.0.1|Add RX power gain option |13/09/24|

#

# Introduction

miotyGO project provides  the user the capability to implement a development mioty **Base Station** for testing purposes, users will be able to run up to 10 mioty endpoints with miotyGO Base Station.

No commercial use allowed.

# Capabilities

miotyGO works as a base station in a mioty rig. After the software installation, miotyGO will be able to:

- Connect with any Service Center compatible with BSSCI 0.9 or 1.0 specification.
- Receive Uplinks from up to 10 mioty endpoints.

# Hardware

miotyGo is available for different platforms such as aarch64 (Raspberry Pi) or x86_64 such as a personal computer running LINUX. In order to be able to receive the uplink, you will need a receiver device, in this case, miotyGO works with SDRPlay RSP1A device.

**Hardware required:**

- Computer/Raspberry Pi 3-4 or 5
- SDRPlay RSP1A
- Antenna for the used frequency band.

# Installation

miotyGO installation is made by running a self extracting installation script. That script provides all necessary resources to be able to run miotyGO on the chosen device.

## Get shell access

First step is to get shell access to your device. You will need to know which is the IP address of your device, in order to discover the IP address of your device, there are different methods you can use.

**Router Interface:**

Access your router's web interface through a web browser. The address is typically something like 192.168.1.1 or 192.168.0.1. Check your router documentation for the exact address.

- Log in to the router using your username and password.
- Look for a section called "Connected Devices," "Device List," or similar. This section usually lists all devices connected to your network along with their IP addresses.

Another method you can use is to run an ARP scan with programs like nmap or arp-scan, this is an example about how to use nmap to discover the IP address of your device.

- Open a terminal on your computer .
- Use the following command to update the package lists:

Update the repository where you will download packages

```shell
sudo apt update
# Install nmap if it's not already installed:
sudo apt install nmap
```

Once nmap is installed, you can use it to scan your local network and find the IP address of your Raspberry Pi. Replace 192.168.1.0/24 with the appropriate network range for your home network:

```shell
sudo nmap -sn 192.168.1.0/24
```

This command performs a ping scan (-sn option) on the specified network range.

Look for a device with a recognizable MAC address or hostname that corresponds to your device.

Once you know the IP address of your device, you can get shell access by establishing ssh connection with the device.

```shell
# Replace user and hostname with yours.
ssh pi@192.168.1.10
```

After running this command you should have shell access on your device.

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.003.png)|
| :- |
|*Figure 1. Run command “uname -a”*|

## Run the installer

Navigate to the path where the installer is placed and execute it (remember you will need to install it with sudo permissions).

```shell
# Navigate until directory where install script is stored

pi@raspberrypi:~ cd $HOME

# Execute the installer
pi@raspberrypi:~ $ sudo ./install.sh
> Extracting Mioty files ...
>>> Found payload, checking integrity ... done
>>> Extracting to /opt/miotyGO...  done
> Checking requirements ... ... ... done
> Installing daemon service ...

BASE STATION SOFTWARE SUCCESSFULLY INSTALLED:

>> This is the unique identifier for your base station: d00000ffff000000
>> Now please register the base station on service center and dowload the certificates
>> Fill the certificates content on /etc/miotyGO/certificates directory as shown:
>>>> ROOT CA Certificate ->  /etc/miotyGO/certificates/root_ca.cer
>>>> TLS Certificate     ->  /etc/miotyGO/certificates/tls.cer
>>>> TLS Key             ->  /etc/miotyGO/certificates/tls.key

>> Once you complete the steps you can start the base station service by running:

>>>> mioty_base_station start

>> You can stop the base station running:

>>>> mioty_base_station stop
```

At this point miotyGO software is already installed in your device’s file system, now you will need to register the miotyGO device in the Service Center.

## Service Center configuration

miotyGO installer will deploy a configuration file in JSON format on **/etc/miotyGO/config.json**, in this file, user should fill with the chosen configuration, this configuration involves three important points:

- Service Center host
- Service Center port
- RX power gain
- Mioty Region

RX power gain is set as default to 100, but can be modified depending of the requirements of the test and the hardware used.

Open **/etc/miotyGO/config.json** and fill your own configuration. These are the available choices for Mioty regions:

- eu868
- eu433
- us915w
- in866

## Register miotyGO on Service Center

miotyGO **Base Station** needs to establish TLS connection with the **Service Center** so you will need to install the certificates provided by the **Service Center** on the miotyGO software.

There are three certificates miotyGO needs, the root CA, the TLS certificate (tls.cer) and the private key (tls.key). These certificates are placed under **/etc/miotyGO/certificates** path.

```shell
DIRECTORY: /etc/miotyGO

├── certificates
│   ├── root_ca.cer          -> root ca certificate
│   ├── tls.cer               -> tls cerfificate
│   └── tls.key               -> tls private key
├── config.json               -> user configuration file
├── options                   -> arguments for base station binary
```

```shell
DIRECTORY: /usr/local/sbin/
├── mioty_bs                  -> base station binary
```

```shell
DIRECTORY: /opt/miotyGO
├── prev                      -> old miotyGO installation files
```

```shell
DIRECTORY: /usr/share/doc/miotyGO
└── how_to_run_miotyGO.md     -> readme doc

```

You need to fill the certificate content for these three files with the certificates generated by your Service Center.

In order to generate the certificates on the Service Center, you need to register the miotyGO device as a base center. To achieve it, you will need the device EUI, this identifier is printed on the shell when you execute the installation script.

For this example, the Service Center used will be **eu3.loriot.io**, but you can use other **Service Center** as well.

You should register miotyGO as a **Mioty Base Station** on the chosen **Service Center**, here is an example of how to do in in **eu3.loriot.io**

### Step 1: Create a new mioty network

Navigate until you **Service Center** provider and create a new mioty network.

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.004.png)|
| :- |
|*Figure 2. Adding a new Mioty network.*|

You will choose the name of the network, in this case **miotyGO** network.

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.005.png)|
| :- |
|*Figure 3. Insert Mioty network name.*|

### Step 2: Add a Base Station

Once the mioty network is created, you should associate a new base station to the network you recently created.

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.006.png)|
| :- |
|*Figure 4. Add a base station to Mioty network.*|

### Step 3: Fill the Base Station EUI with miotyGO EUI

Introduce the base station EUI, you will find this ID printed on your raspberry pi once you run the installer.  

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.007.png)|
| :- |
|*Figure 5. Fill Base Station EUI.*|

You will see the base station available but remains offline.￼  

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.008.png)|
| :-: |
|*Figure 6. Base station online.*|

### Step 4: Fill the certificates

In order to establish TLS communication **Base Station-Service Center** you will need to generate the certificates required by the miotyGO base station, you can do this on your service center provider as shown in Figure 6 - 7 - 8.

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.009.png)|
| :-: |
|*Figure 7. Generate the certificates for the base station.*|

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.010.png)|
| :- |
|*Figure 8. Root CA certificate.*|

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.011.png)|
| :- |
|*Figure 9. Tls certificate and key.*|

Once you generate the certificates, you will need to fill the content on the Raspberry Pi, you can do this by opening these files with your favorite text editor on the Raspberry Pi.

```shell
pi@raspberrypi:~ $ vim /etc/miotyGO/certificates/root_ca.cer

pi@raspberrypi:~ $ vim /etc/miotyGO/certificates/tls.cer

pi@raspberrypi:~ $ vim /etc/miotyGO/certificates/tls.key
```

Once you fill the content of the three certificates, you will be able to start the miotyGO Base Station service by running next command:

```shell
pi@raspberrypi:~ $ sudo mioty_base_station start
```

If the installation went well, miotyGO **Base Station** will connect to the **Service Center**.

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.012.png)|
| :-: |
|*Figure 10. miotyGO online.*|
||
You should be able to see the logs by running this command:

```shell
pi@raspberrypi:~ $ tail -f /var/log/miotyGO.log
```

## Attach a device

At this point, you should get the miotyGO Base Station connected with the Service Center, now you need to attach a device. Following is an example of how to do it at the service center of [eu3.loriot.io](https://eu3.loriot.io).

You will need to create a new application. To do this, navigate under applications and create a new mioty application.

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.013.png)|
| :-: |
|*Figure 11. Create an application.*|

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.014.png)|
| :-: |
|*Figure 12. Mioty application.*|

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.015.png)|
| :- |
|<p>*Figure 13. miotyGO application name.*</p><p></p>|
Once you got the mioty application created, you will need to create a new device.

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.016.png)|
| :-: |
|*Figure 14. miotyGO new device.*|

Inside this device, enroll an endpoint with the parameters given by the manufacturer.

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.017.png)|
| :-: |
|*Figure 15. Enroll new endpoint.*|

Once the endpoint is already enrolled, propagate the attachment with the miotyGO **Base Station.**

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.018.png)|
| :-: |
|*Figure 16. New device attached.*|

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.019.png)|
| :-: |
|*Figure 17. Propagate new attachment.*|

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.020.png)|
| :-: |
|*Figure 18. Propagate attachment on base station.*|

If the propagation was successful, you should see the log on miotyGO logs.

|![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.021.png)|
| :-: |
|*Figure 19. New endpoint attached.*|

V.1.0           ![](docs/img/Aspose.Words.0e6b2a72-e5d0-43dc-8ba1-6111543b9273.022.png)
