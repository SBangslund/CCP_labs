### 1.1 Keypairs
Using the local unix command: ```ssh-keygen -t rsa -b 2048```, the keys are generated in ~/.ssh/* (both id_rsa and .pub). These are then imported in the Openstack compute -> key pairs as Desktop.

### 1.2 Security Groups
Created a new group called Allow SSH and HTTP. This extends the default with ICMP, SSH (22), and HTTP (80) - the ICMP is for pinging.

### 1.3 Understanding Quotas
Based on the restrictions this Openstack instance will only allow for 6 virtual CPUs, which means, that at max 6 virtual machines would be running. These would have to share 8 GB of RAM and 1000 GB of harddisk.

### 1.4 Create your VM
Created a new VM instance with the required image: '2019-08_Ubuntu_Bionic_with_Docker'. This also contains the 'm1.small' flavor, which provides it with 2GB of RAM, 1 VCPU and a disk size of 120GB. It uses the previously configured security group ('Allow SSH and HTTP').

The floating IP is: 160.85.37.78, and the SSH key 'Desktop' has been included - which enables an SSH connection from the start.

### 1.5 Basic VM Management
No, the IP assigned is not the same. The floating IP is the public IP, that is accessible from the outside of the network. Because of NAT, this is associated with the local IP - the one the VM gets configured.

Yes, the file is still present every time. This is due to the external volume was where we saved the file - not on the instance. When creating a snapshot, we take a snapshot of the current volume, this can then be invoked as a new instance which is very useful for backing up instances. Can be seen like git and its commits.

### 2.1 Networking
Made a network called local with the subnet 192.168.0.0/24, and a router for 192.168.0.1, with a DHCP in the range 192.168.0.100-150.

### 2.2 Virtual Machine
Made a _network machine with the new network assigned. Associating the floating IP made the SSH part identical.

### 2.3 Volumes
The volume is easily associated, formatted and mounted as instructed. The volume also keeps the data when unmounted etc.

Listing disks: ```fdisk -l```

Formatting disk: ```mkfs.ext4 $DEVICE_NAME```

Mounting disk: ```mount $DEVICE_NAME $DIRECTORY ```

Yes it can get extended, after removing attachment to instance. It also contains the data from before the extension. Some advantages could be, that the snapshot is from the whole system, where this is only the data on that particular volume. 

### 2.4 Management
The resource id is 5d19dd40-8720-476b-b91c-a91d94d36cf1. It is useful for referencing this particular instance as it is unique (UUID).

It has been assigned 192.168.0.118 from the DHCP - which is from the subnet. It is not the same as the floating IP, as it is not public.

There is a lot of metadata of the instance (VM). Age, creation date, status, zone, specs, security groups, attached volumes, ssh keys, etc. 

### 2.5 VM Scaling
The difference between vertical and horizontal scaling is:
- vertical scaling refers to scaling of the vm itself. This could be adjusting the specs of the machine (or volumes attached).
- horizontal scaling is adding more machines without any problems. Like taking a snapshot an duplicating a VM for more power.

Vertical scaling can be useful for monolithic dependent applications and the hosting thereof. This can sometimes demand a more from the host.

### 3.0 Cleanup
The resources should be cleanup to ease on the bill, these resources include:
- Delete volumes
- Remove the SSH keys ad Security Groups and rules that you created.
- Delete the Network and Subnet.
- Release the Floating IPs.
- Delete the Router.
- Delete the VM.

In which order these are deleted is important as some of them depends on eachother (through association).