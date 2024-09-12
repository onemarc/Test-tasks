<aside>

## 🌐 Networking

</aside>
I described the step-by-step creation of a VPC with two subnets and tested their ability to communicate with each other. For clarity, I checked the possibility to connect from any network using the default VPC, which is created automatically in Google Cloud Platform.

### 1. Create a custom VPC network with subnets

1.1 Creation of custom VPC network **privatenet** with subnets: **privatesubnet-us** and **privatenet-eu**

```bash
gcloud compute networks create privatenet --subnet-mode=custom

gcloud compute networks subnets create privatesubnet-us \
	--network=privatenet \
	--region=us-east4 \
	--range=172.16.0.0/24

gcloud compute networks subnets create privatesubnet-eu \
	--network=privatenet \
	--region=europe-central2 \
	--range=172.20.0.0/20
```

![image](https://github.com/user-attachments/assets/48f199dd-3543-4472-91e0-0553fbd9c243)


To view the creation of the VPC network and its subnets, I used the commands below.

```bash
gcloud compute networks list

gcloud compute networks subnets list --sort-by=NETWORK
```

### 2. Create the firewall rules for **privatenet** network

Create a firewall rule to allow inbound traffic to virtual machine instances on the **privatenet** network via SSH, ICMP, and RDP.

```bash
gcloud compute firewall-rules create privatenet-allow-icmp-ssh-rdp \
	--direction=INGRESS \
	--priority=1000 \
	--network=privatenet \
	--action=ALLOW \
	--rules=icmp,tcp:22,tcp:3389 \
	--source-ranges=0.0.0.0/0
```

![image](https://github.com/user-attachments/assets/3d1b584f-3269-4370-b255-e05d8742ad78)


## **3. Creation of VM Instances**

### 3.1 Create a **privatenet-us-instance** in the **privatesubnet-us**

```bash
gcloud compute instances create privatenet-us-instance \
	--zone=us-east4-c \
	--machine-type=e2-micro \
	--subnet=privatesubnet-us
```

![image](https://github.com/user-attachments/assets/b0857a39-d3f4-4c85-b4ca-c420dba84b69)


### 3.2 Create a **privatenet-eu-instance** in the **privatesubnet-eu**

```bash
gcloud compute instances create privatenet-eu-instance \
	--zone=europe-central2-c \
	--machine-type=e2-micro \
	--subnet=privatesubnet-eu
```

![image](https://github.com/user-attachments/assets/e5c2563c-fd56-40a2-b726-e442492851ab)


### 3.3 Creation of vm instances in the default VPC in the US and EU regions.

I created vm instances in the default VPC to test connectivity between instances in the privatenet-us and privatenet-eu VPCs.

```bash
gcloud compute instances create my-us-instance \
	--zone=us-east4-c \
	--machine-type=e2-micro \
	--subnet=default
	
gcloud compute instances create my-eu-instance \
	--zone=europe-central2-c \
	--machine-type=e2-micro \
	--subnet=default
```

## 4. Test the instances

### 4.1 Test the external IP address for all VM instances with my-us-instance and my-eu-instance

To do this, I connected to the vm instance via ssh and used the **`ping`** command to check the possibility of accessing different instances from the default VPC via the external IP.

```bash
ping -c 3 “external-ip-address”
```

![image](https://github.com/user-attachments/assets/48e44969-1dff-4a19-8e8f-bada437033e3)


Check the external IP from the **my-us-instance**:
![image](https://github.com/user-attachments/assets/bf7accec-d058-411a-86ce-d0948f47ff9c)


Check the external IP from the **my-eu-instance**:
![image](https://github.com/user-attachments/assets/481bf2aa-75dc-42d8-852c-23109ffd46ef)


You can check the external IP address of all virtual machine instances, even if they are in a different zone or VPC network. This confirms that public access to these instances is controlled only by the ICMP firewall rules that were set earlier.

### 4.2 Test the internal IP address of all VM instances from my-us-instance and my-eu-instance

![image](https://github.com/user-attachments/assets/2c6158bd-f6a2-4907-bfe0-dd0a8674810e)


### 4.3 Test the internal IP address from the privatenet-us-instance

Check the internal IP from the **privatenet-us-instance**:
![image](https://github.com/user-attachments/assets/05516cb4-635d-4a5e-9d4e-8e1585fa5ae6)

![image](https://github.com/user-attachments/assets/1ddfc320-051f-4a61-9319-b155b9edf875)

Check the internal IP from the **privatenet-eu-instance**:

![image](https://github.com/user-attachments/assets/69547f4a-bcc1-416d-bceb-c3ee28d616c2)

![image](https://github.com/user-attachments/assets/cf226e20-4ca2-4a7b-bb1b-9a2133485818)

You can check the internal IP address of **private-us-instance** because it is in the same VPC network as the ping source, namely **private-eu-instance**, even though both VM instances are in different zones and regions . The situation with **privatenet-us-instance** is identical.

By default, VPC networks are isolated private network domains. However, sharing internal IP addresses between networks is prohibited unless you configure VPC peering or VPN.

## 5. Network troubleshooting

To debug the network connection, I would first pay attention to the following aspects and follow these steps:

1. First, you should check the logs, using Cloud Logging, for network-related errors.
2. Check the virtual machine firewall rules to ensure that the firewall rules you configured allow traffic between virtual machines.
3. Verify that both virtual machines are on the correct subnets and that their internal IP addresses are assigned correctly.
4. If using VPC peering or a shared VPC to connect between virtual machines, ensure that the peering and required routes are configured correctly.
5. If network tags are applied to virtual machines, you need to check whether they comply with the firewall rules that have been created.
