# Roger-skyline

The project has been done on Debian 9.8.0

### Step 1 - Installing Debian VM
----------------------------------------
Install Debian to your VM (VirtualBox in my case). Select 8GB the total disk space, create 4.2GB partition.
If you faced issue with software intallation, try not to install GUI interface. You can try to download and install it later. If it is not possible, use Debian without any GUI interface.

When installing you will be prompted to set root password along with non-root user_name and user_password. Remember this information, you will need it later.

### Step 2 - Network and Security
-------------------------------------
In order to install all necessary utilities, run:

`apt-get install -y sudo net-tools iptables-persistent fail2ban sendmail apache2 cron`

Open `/etc/ssh/sshd_config` file using nano editor, change the lines as follows:

`Port 6969
 PasswordAuthentication yes
 PermitRootLogin no
 PubkeyAuthentication yes`


