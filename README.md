# Configure-Multiple-Elastic-IPs-and-Attach-multiple-Elastic-IPs-with-a-Linux-server
Step 1: Allocate Elastic IPs
Go to AWS Console → Open EC2 Dashboard.
In the left panel, click Elastic IPs.
Click Allocate Elastic IP Address.
Choose Amazon's pool of IPv4 addresses and click Allocate.
Repeat this process to allocate multiple Elastic IPs.
________________________________________________________________________________________________________________________________________________________________
Step 2: Attach the First Elastic IP to Primary Network Interface
In the Elastic IPs section, select an Elastic IP.
Click Actions → Associate Elastic IP Address.
Select your EC2 instance.
Choose the Primary network interface (eth0).
Click Associate.
✅ Now, the first Elastic IP is attached to your primary network interface.
________________________________________________________________________________________________________________________________________________________________
Step 3: Create a Secondary Network Interface
Go to EC2 Dashboard → Click Network Interfaces (on the left panel).
Click Create network interface.
Choose the same VPC and subnet as your EC2 instance.
Assign a private IP address (e.g., 192.168.1.20).
Click Create.
________________________________________________________________________________________________________________________________________________________________
Step 4: Attach the Secondary Network Interface to EC2
In Network Interfaces, select the new interface.
Click Actions → Attach to instance.
Select your EC2 instance and click Attach.
✅ Now, your EC2 instance has two network interfaces.
________________________________________________________________________________________________________________________________________________________________
Step 5: Associate the Second Elastic IP to the New Interface
Go to Elastic IPs and select another Elastic IP.
Click Actions → Associate Elastic IP Address.
Choose Network Interface (select the one you just created).
Click Associate.
✅ Now, the second Elastic IP is attached to the secondary network interface.
________________________________________________________________________________________________________________________________________________________________
Step 6: Configure Linux to Use Both IPs
Connect to your EC2 instance via SSH:
sh
Copy
Edit
ssh -i your-key.pem ec2-user@your-primary-elastic-ip
Check existing network interfaces:
sh
Copy
Edit
ip a
Add a secondary IP manually (replace eth1 with your second interface):
sh
Copy
Edit
sudo ip addr add 192.168.1.20/24 dev eth1
Make it permanent by editing network configuration:
Open the interface config file:
sh
Copy
Edit
sudo nano /etc/network/interfaces
Add:
nginx
Copy
Edit
auto eth1
iface eth1 inet static
    address 192.168.1.20
    netmask 255.255.255.0
Save and exit (Ctrl + X, then Y, then Enter).
Restart networking:
sh
Copy
Edit
sudo systemctl restart networking
________________________________________________________________________________________________________________________________________________________________
Step 7: Test Both IPs
Ping both Elastic IPs from another system:
sh
Copy
Edit
ping <Elastic-IP-1>
ping <Elastic-IP-2>
Check public IPs on your EC2 instance:
sh
Copy
Edit
curl ifconfig.me
✅ Done! Your EC2 instance is now using multiple Elastic IPs successfully. 🚀
________________________________________________________________________________________________________________________________________________________________
