# Roger-skyline

The project has been done on Debian 9.8.0

### Step 1 - Installing Debian VM
----------------------------------------
Install Debian to your VM (VirtualBox in my case). Select 8GB the total disk space, create 4.2GB partition.
If you faced issue with software installation, try not to install GUI interface. You can try to download and install it later. If it is not possible, use Debian without any GUI interface.

When installing you will be prompted to set root password along with non-root user_name and user_password. Remember this information, you will need it later.

### Step 2 - Network and Security
-------------------------------------
* In order to install all necessary utilities, run:

`apt-get install -y sudo net-tools iptables-persistent fail2ban sendmail apache2 cron`

* Open `/etc/ssh/sshd_config` file using nano editor, change the lines as follows:

`Port 6969`

`PasswordAuthentication yes`
 
 `PermitRootLogin no`
 
 `PubkeyAuthentication yes`
 
 * Open `/etc/network/interfaces` in nano editor, edit the file as follows:
 
 `source /etc/network/interfaces.d/*`
  
 ` #The loopback network interface`
  
  `auto lo`
  
  `iface lo inet loopback`
  
 ` allow-hotplug enp0s3`
  
  `iface enp0s3 inet dhcp`
  
  `allow-hotplug enp0s8`
  
  `iface enp0s8 inet static address 192.168.56.3\30`
  
  * Make user as root, open `/etc/passwd` , find user name and change `UID` and `GID` to `0`
  
  * Run `ssh-keygen`, run `cat ~/.ssh/id_rsa.pub` and copy the entire contents.
  
  * Make conection to the server: `ssh <root-user>@debian -p 6969`
  
  * Create ssh directory `mkdir .ssh`
  
  * Paste the key into .ssh/authorized_keys
  
  * In the `/etc/ssh/sshd_config` change line as follows: `PasswordAuthentication no`
  
  * Run `sudo service ssh restart`
  
  * Run `sudo nano /etc/network/if-pre-up.d/iptables` and paste the following in order to set a firewall:
  
  `#!/Bin/bash`
  
    `iptables-restore </etc/iptables.test.rules`
    
    iptables -F iptables -X iptables -t nat -F iptables -t nat -X iptables -t mangle -F iptables -t mangle -X
    
    iptables -P INPUT DROP
    
    iptables -P OUTPUT DROP
    
    iptables -P FORWARD DROP
    
    iptables -A INPUT -m conntrack -ctstate ESTABLISHED, RELATED -j ACCEPT
    
    iptables -A INPUT -p tcp -i enp0s8 -dport 2222 -j ACCEPT
    
    iptables -A INPUT -p tcp -i enp0s8 -dport 80 -j ACCEPT
    
    iptables -A INPUT -p tcp -i enp0s8 -dport 443 -j ACCEPT
    
    iptables -A OUTPUT -m conntrack! --ctstate INVALID -j ACCEPT
    
    iptables -I INPUT -i lo -j ACCEPT
    
    iptables -A INPUT -j LOG
    
    iptables -A FORWARD -j LOG
    
    iptables -I INPUT -p tcp -dport 80 -m connlimit -connlimit-above 10 -connlimit-mask 20 -j DROP
    
    #port scan
    
    iptables -N port-scanning
    
    iptables -A port-scanning -p tcp --tcp-flags SYN,ACK,FIN,RST RST -m limit --limit 60/s --limit-burst 2 -j RETURN`
    
    `iptables -A port-scanning -j DROP`
    
    
   
 
    
    
    
    
    
   


