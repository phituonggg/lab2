# Lab2 

#  21110108, Tran Phi Tuong, Information Security_ Nhom 03FIE
# link GitHub : https://github.com/phituonggg/lab2/blob/main/21110108%2C%20Tran%20Phi%20Tuong.txt

Lab #2, Firewall Configuration and Encryption
Task 1: Firewall Configuration
Question 1: Network Configuration with 2 Subnets and a Router
Answer 1:
1. Configure Subnet 1 (192.168.1.0/24):
   - Router interface on eth0: 192.168.1.1
   - Web server (PC0) on 192.168.1.2
   
2. Configure Subnet 2 (192.168.2.0/24):
   - Router interface on eth1: 192.168.2.1
   - Client workstations: PC1 on 192.168.2.2, PC2 on 192.168.2.3

3. Router configuration:
   - Router interfaces eth0 and eth1 for Subnets 1 and 2.
   - Enable IP forwarding on the router to route traffic between subnets.

4. Web Server (PC0) Configuration:
   - Install a simple web server like Apache on PC0 to serve content.
   - The web server's IP will be 192.168.1.2.

5. Client Configuration (PC1 and PC2):
   - Set static IPs for PC1 (192.168.2.2) and PC2 (192.168.2.3).
Question 2: Enabling Packet Forwarding and SSH Defacement
Answer 2:
1. Enable packet forwarding on the router by editing the sysctl configuration or using the following command:
```sh
sysctl -w net.ipv4.ip_forward=1
```
2. To deface the webserver's home page using SSH, login from PC1 using the following command:
```sh
ssh user@192.168.1.10
```
3. After connecting, navigate to the webserver's document root and modify the home page file.
Question 3: Block SSH to Web Server from PC1 on Router
Answer 3:
To block SSH access from PC1 (192.168.2.10) to the webserver (PC0) while allowing access from other hosts, configure the router's firewall rules as follows:
```sh
iptables -A INPUT -p tcp --dport 22 -s 192.168.2.10 -j DROP
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```
This will drop all SSH packets from PC1 while allowing SSH access from other hosts.
Question 4: UDP Server on PC1 and Personal Firewall Configuration
Answer 4:
1. Set up PC1 as a UDP server to reply to ping requests from other hosts on both subnets by running a UDP listener (e.g., using netcat):
```sh
nc -u -l 12345
```
2. To configure PC1's firewall to block UDP access from PC2 (192.168.2.20) while allowing access from the server, use the following iptables rule:
```sh
iptables -A INPUT -p udp -s 192.168.2.20 -j DROP
```
Task 2: Encrypting Large Message
Question 1: AES-Cipher in CTR and OFB Modes
Answer 1:
1. Encrypt the text file using AES in CTR and OFB modes with OpenSSL:
- CTR Mode:
```sh
openssl enc -aes-256-ctr -in largefile.txt -out encrypted_ctr.bin -K <key> -iv <iv>
```
- OFB Mode:
```sh
openssl enc -aes-256-ofb -in largefile.txt -out encrypted_ofb.bin -K <key> -iv <iv>
```
In terms of error propagation, CTR mode does not propagate errors, while in OFB mode, only the bit where the error occurred is affected.
2. To send the file with message authentication, use HMAC or another integrity check mechanism. Example with HMAC:
```sh
openssl dgst -sha256 -hmac <key> -out hmac.bin largefile.txt
```
Question 2: Corrupted File Verification
Answer 2:
When the 6th bit of the encrypted file is corrupted, the behavior will differ depending on the mode:
- In CTR mode, the decryption will result in errors in only the corrupted block.
- In OFB mode, the corrupted bit will only affect the corresponding decrypted bit.
Question 3: Decryption of Corrupted Files
Answer 3:
After decrypting the corrupted files:
- In CTR mode, error propagation does not extend beyond the corrupted block.
- In OFB mode, the decryption is bitwise, and error propagation is minimal. This makes OFB less sensitive to bit errors.
