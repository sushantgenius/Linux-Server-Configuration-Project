# Linux Server Configuration

This is the final project for Udacity's [Full Stack Web Developer Nanodegree](https://www.udacity.com/course/full-stack-web-developer-nanodegree--nd004).

I made this final project using the help of Amazon Lightsail
You can use DigitalOcean or Amazon Lightsail
However I recommend DigitalOcean
- The virtual private server is [Amazon Lighsail](https://lightsail.aws.amazon.com/).
- The web application is  [Item Catalog project](https://github.com/boisalai/udacity-catalog-app) created by a fellow coursemate(This is a pilot run).
- The database server is [PostgreSQL](https://www.postgresql.org/).
- My local machine is a Windows 10 (Windows 10 PRO).
- For this project I have installed the GIT Bash to access the remote machine
- You can also use Vagrant and Virtualbox setup to access your remote server ON lightsail

-You can visit 13.232.238.49 or http://http://2dd1763c602c62b6708517937492a65f-2102374274.ap-south-1.elb.amazonaws.com/ for the website deployed.
- You can also setup your own DNS server using popular host provider
- PLEASE NOTE:- To get http:// or https:// certification you need to create Load-Balancing in the Networking section of the lightsail Instance


## Get a server

### Step 1: Create a Ubuntu Linux server instance on Amazon Lightsail

- Login into [Amazon Lightsail](https://lightsail.aws.amazon.com/ls/webapp/home/resources) using an Amazon Web Services account.
- After Logged in Click on 'Create Instance'.
- Choose `Linux/Unix` platform, `OS Only` and  `Ubuntu 16.04 LTS`.
- Choose a instance plan (I took the cheapest, $5/month) (NOTE : IF YOU ARE A STUDENY YOU CAN GET FREE CREDITS ON REQUEST).
- Change the name if you like or keep it the same.
- Click the `Create` button to create the instance.
- Wait for the instance to start up.
- Once started Click on the Networking section of the instance
- Go down to the firewall area and remove all the ports
##Download the SSH private key from the account section at the top right

**Reference**
- ServerPilot, [How to Create a Server on Amazon Lightsail](https://serverpilot.io/community/articles/how-to-create-a-server-on-amazon-lightsail.html).

##Followed by downloading [PuttyGen](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- Install the PuttyGen
- Open PuttyGen
- Click on the load the private keys
- Browse and locate the private SSH key from amazonaws
- After it is done hit open it will generate the public key
- Once the public key is generated copy that keys
##Open Putty
- After generating and saving the PUTTY private key .pkp
- Open Git Bash and run the command cd ssh to check if you have ssh Folder
- Move that .pem private key from Amazon and place it inside the ssh folder
- Open Putty and now put the hostname inside sessions
- Next Go to SSH down below expand it and go it auth
- In the Private key section hit Browse and browse the file private keys .pkp
- Go back to the sessions and now name the session and save that session
- Click open and click on Yes in the dialog box
- Putty terminal will Open
- Type the username as Ubuntu
- SSH key will Automatically load up the system
- Now change the port to 2200
- Type sudo nano /etc/ssh/sshd_config and change port from 22 to 2200
- Once done hit ctrl+x
- Hit Y and again enter to Save
- Type sudo ufw enable to enable firewall
- Now exit the putty terminal
- Open PUTTY and this time chane port to 2200 ans save it as new sessions
- Go to Amazon lightsail and in networking section of the instance
- Change the firewall settings and add port 2200 in it.


### Alternatively you could have used GITBASH Command and SSH key to access the server


### SSH into the server

- From the `Account` menu on Amazon Lightsail, click on `SSH keys` tab and download the Default Private Key.
- Move this private key file named `LightsailDefaultKey-ap-south-1-*.pem` into the local folder `~/.ssh`
- In your terminal, type: `chmod 600 ~/.ssh/LightsailDefaultKey-ap-south-1.pem`.
- To connect to the instance via the terminal: `ssh -i ~/.ssh/LightsailDefaultKey-ap-south-1.pem ubuntu@13.232.238.49`,
  where `13.232.238.49` is the public IP address of the instance.

<!--
Public IP address is 13.232.238.49.
ssh -i ~/.ssh/LightsailDefaultKey-ap-south-1 ubuntu@13.232.238.49
-->

## Secure the server

### Update and upgrade installed packages

```
sudo apt-get update
sudo apt-get upgrade
```


### Change the SSH port from 22 to 2200

- Edit the `/etc/ssh/sshd_config` file: `sudo nano /etc/ssh/sshd_config`.
- Change the port number on line 5 from `22` to `2200`.
- Save and exit using CTRL+X and confirm with Y.
- Restart SSH: `sudo service ssh restart`.

### Configure the Uncomplicated Firewall (UFW)

- Configure the default firewall for Ubuntu to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
  ```
  sudo ufw status                  
  sudo ufw default deny incoming   
  sudo ufw default allow outgoing  
  sudo ufw allow 2200/tcp          
  sudo ufw allow www               
  sudo ufw allow 123/udp          
  sudo ufw deny 22                 
  ```

- Turn UFW on: `sudo ufw enable`. The output should be like this:

- Check the status of UFW to list current roles: `sudo ufw status`. The output should be like this:

  ```
  Status: active

  To                         Action      From
  --                         ------      ----
  2200/tcp                   ALLOW       Anywhere
  80/tcp                     ALLOW       Anywhere
  123/udp                    ALLOW       Anywhere
  22                         DENY        Anywhere
  2200/tcp (v6)              ALLOW       Anywhere (v6)
  80/tcp (v6)                ALLOW       Anywhere (v6)
  123/udp (v6)               ALLOW       Anywhere (v6)
  22 (v6)                    DENY        Anywhere (v6)
  ```

- Exit the SSH connection: `exit`.

- Click on the `Manage` option of the Amazon Lightsail Instance,
then the `Networking` tab, and then change the firewall configuration to match the internal firewall settings above.
  <img src="images/screen4.png" width="600px">

- Allow ports 80(TCP), 123(UDP), and 2200(TCP), and deny the default port 22.
  <img src="images/screen5.png" width="600px">

- From your local terminal, run: `ssh -i ~/.ssh/LightsailDefaultKey-ap-south-1 -p 2200 ubuntu@13.232.238.49`, where `13.232.238.49` is the public IP address of the instance.

<!--
Public IP address is 13.232.238.49.
ssh -i ~/.ssh/LightsailDefaultKey-ap-south-1 -p 2200 ubuntu@13.232.238.49
-->

**References**
- Official Ubuntu Documentation, [UFW - Uncomplicated Firewall](https://help.ubuntu.com/community/UFW).
- TechRepublic, [How to install and use Uncomplicated Firewall in Ubuntu](https://www.techrepublic.com/article/how-to-install-and-use-uncomplicated-firewall-in-ubuntu/).





### Updated packages to most recent versions


Some packages have not been updated because the server need to be rebooted. I found the solution [here](https://www.digitalocean.com/community/questions/updating-ubuntu-14-04-security-updates).

- I did these commands:
  ```
  sudo apt-get update
  sudo apt-get dist-upgrade
  sudo shutdown -r now
  ```

- Logged back in, and I now see this message:
  ```
  Alains-MBP:udacity-linux-server-configuration boisalai$ ssh -i ~/.ssh/LightsailDefaultKey-ap-south-1 -p 2200 ubuntu@13.232.238.49
  Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.4.0-1039-aws x86_64)

   * Documentation:  https://help.ubuntu.com
   * Management:     https://landscape.canonical.com
   * Support:        https://ubuntu.com/advantage

    Get cloud support with Ubuntu Advantage Cloud Guest:
      http://www.ubuntu.com/business/services/cloud

  0 packages can be updated.
  0 updates are security updates.



**Reference**
- DigitalOcean, [Updating Ubuntu 14.04 -- Security Updates](https://www.digitalocean.com/community/questions/updating-ubuntu-14-04-security-updates).



## Give `grader` access


### Create a new user account named `grader`

- While logged in as `ubuntu`, add user: `sudo adduser grader`.
- Enter a password (twice) and fill out information for this new user.
#### H4
PASSWORD TO THE grader and catalog user is "KIIT"


### Give `grader` the permission to sudo

- Edits the sudoers file: `sudo visudo`.
- Search for the line that looks like this:
  ```
  root    ALL=(ALL:ALL) ALL
  ```

- Below this line, add a new line to give sudo privileges to `grader` user.
  ```
  root    ALL=(ALL:ALL) ALL
  grader  ALL=(ALL:ALL) ALL
  ```

- Save and exit using CTRL+X and confirm with Y.
- Verify that `grader` has sudo permissions. Run `su - grader`, enter the password,
run `sudo -l` and enter the password again. The output should be like this:

  ```
  Matching Defaults entries for grader on ip-172-26-13-170.us-east-2.compute.internal:
      env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

  User grader may run the following commands on ip-172-26-13-170.us-east-2.compute.internal:
      (ALL : ALL) ALL
  ```

**Resources**
- DigitalOcean, [How To Add and Delete Users on an Ubuntu 14.04 VPS](https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-an-ubuntu-14-04-vps)


### Create an SSH key pair for `grader` using the `ssh-keygen` tool

- On the local machine:
  - Run `ssh-keygen`
  - Enter file in which to save the key (I gave the name `grader_key`) in the local directory `~/.ssh`
  - Enter in a passphrase twice. Two files will be generated (  `~/.ssh/grader_key` and `~/.ssh/grader_key.pub`)
  - Run `cat ~/.ssh/grader_key.pub` and copy the contents of the file
  - Log in to the grader's virtual machine
- On the grader's virtual machine:
  - Create a new directory called `~/.ssh` (`mkdir .ssh`)
  - Run `sudo nano ~/.ssh/authorized_keys` and paste the content into this file, save and exit
  - Give the permissions: `chmod 700 .ssh` and `chmod 644 .ssh/authorized_keys`
  - Check in `/etc/ssh/sshd_config` file if `PasswordAuthentication` is set to `no`
  - Restart SSH: `sudo service ssh restart`
- On the local machine, run: `ssh -i ~/.ssh/grader_key -p 2200 grader@13.232.238.49`.

<!--
Public IP address is 13.232.238.49.
ssh -i ~/.ssh/grader_key -p 2200 grader@13.232.238.49
le paraphrase est grader
-->

**References**
- DigitalOcean, [How To Set Up SSH Keys](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2).
- Ubuntu Wiki, [SSH/OpenSSH/Keys](https://help.ubuntu.com/community/SSH/OpenSSH/Keys).




## Prepare to deploy the project

### Configure the local timezone to UTC

- While logged in as `grader`, configure the time zone: `sudo dpkg-reconfigure tzdata`. You should see something like that:



**References**
- Ubuntu Wiki, [UbuntuTime](https://help.ubuntu.com/community/UbuntuTime)
- Ask Ubuntu, [How do I change my timezone to UTC/GMT?](https://askubuntu.com/questions/138423/how-do-i-change-my-timezone-to-utc-gmt/138442)



### Install and configure Apache to serve a Python mod_wsgi application

- While logged in as `grader`, install Apache: `sudo apt-get install apache2`.
- Enter public IP of the Amazon Lightsail instance into browser. If Apache is working, you should see:
  <img src="images/screen6.png" width="600px">

- My project is built with Python 3. So, I need to install the Python 3 mod_wsgi package:
 `sudo apt-get install libapache2-mod-wsgi-py3`.
- Enable `mod_wsgi` using: `sudo a2enmod wsgi`.


### Install and configure PostgreSQL

- While logged in as `grader`, install PostgreSQL:
 `sudo apt-get install postgresql`.
- PostgreSQL should not allow remote connections. In the  `/etc/postgresql/9.5/main/pg_hba.conf` file, you should see:
  ```
  local   all             postgres                                peer
  local   all             all                                     peer
  host    all             all             127.0.0.1/32            md5
  host    all             all             ::1/128                 md5
  ```

- Switch to the `postgres` user: `sudo su - postgres`.
- Open PostgreSQL interactive terminal with `psql`.
- Create the `catalog` user with a password and give them the ability to create databases:
  ```
  postgres=# CREATE ROLE catalog WITH LOGIN PASSWORD 'catalog';
  postgres=# ALTER ROLE catalog CREATEDB;
  ```

- List the existing roles: `\du`. The output should be like this:
  ```
                                     List of roles
   Role name |                         Attributes                         | Member of
  -----------+------------------------------------------------------------+-----------
   catalog   | Create DB                                                  | {}
   postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
  ```

- Exit psql: `\q`.
- Switch back to the `grader` user: `exit`.
- Create a new Linux user called `catalog`: `sudo adduser catalog`. Enter password and fill out information.
- Give to `catalog` user the permission to sudo. Run: `sudo visudo`.
- Search for the lines that looks like this:
  ```
  root    ALL=(ALL:ALL) ALL
  grader  ALL=(ALL:ALL) ALL
  ```

- Below this line, add a new line to give sudo privileges to `catalog` user.
  ```
  root    ALL=(ALL:ALL) ALL
  grader  ALL=(ALL:ALL) ALL
  catalog  ALL=(ALL:ALL) ALL
  ```

- Save and exit using CTRL+X and confirm with Y.
- Verify that `catalog` has sudo permissions. Run `su - catalog`, enter the password, run `sudo -l` and enter the password again. The output should be like this:

  ```
  Matching Defaults entries for catalog on ip-172-26-13-170.us-east-2.compute.internal:
      env_reset, mail_badpass,
      secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

  User catalog may run the following commands on ip-172-26-13-170.us-east-2.compute.internal:
      (ALL : ALL) ALL
  ```

- While logged in as `catalog`, create a database: `createdb catalog`.
- Run `psql` and then run `\l` to see that the new database has been created. The output should be like this:
  ```
                                    List of databases
     Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
  -----------+----------+----------+-------------+-------------+-----------------------
   catalog   | catalog  | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
   postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
   template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
             |          |          |             |             | postgres=CTc/postgres
   template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
             |          |          |             |             | postgres=CTc/postgres
  (4 rows)
  ```
- Exit psql: `\q`.
- Switch back to the `grader` user: `exit`.

**Reference**
- DigitalOcean, [How To Secure PostgreSQL on an Ubuntu VPS](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps).



### Install git

- While logged in as `grader`, install `git`: `sudo apt-get install git`.

## Deploy the Item Catalog project

### Clone and setup the Item Catalog project from the GitHub repository

- While logged in as `grader`, create `/var/www/catalog/` directory.
- Change to that directory and clone the catalog project:<br>
`sudo git clone https://github.com/boisalai/udacity-catalog-app.git catalog`.
- From the `/var/www` directory, change the ownership of the `catalog` directory to `grader` using: `sudo chown -R grader:grader catalog/`.
- Change to the `/var/www/catalog/catalog` directory.
- Rename the `application.py` file to `__init__.py` using: `mv application.py __init__.py`.

- In `__init__.py`, replace line 27:
  ```
  # app.run(host="0.0.0.0", port=8000, debug=True)
  app.run()
  ```

- In `database.py`, replace line 9:
   ```
   # engine = create_engine("sqlite:///catalog.db")
   engine = create_engine('postgresql://catalog:PASSWORD@localhost/catalog')
   ```

### Authenticate login through Google

- Go to [Google Cloud Plateform](https://console.cloud.google.com/).
- Click `APIs & services` on left menu.
- Click `Credentials`.
- Create an OAuth Client ID (under the Credentials tab), and add http://13.59.39.163 and
http://ec2-13-59-39-163.us-east-2.compute.amazonaws.com as authorized JavaScript
origins.
- Add http://ec2-13-59-39-163.us-east-2.compute.amazonaws.com/oauth2callback
as authorized redirect URI.
- Download the corresponding JSON file, open it et copy the contents.
- Open `/var/www/catalog/catalog/client_secret.json` and paste the previous contents into the this file.
- Replace the client ID to line 25 of the `templates/login.html` file in the project directory.

<!--
### Authenticate login through Facebook

- Go to [Facebook for Developers](https://developers.facebook.com/).
- Click `My Apps` and click `Add a New App`.
- Enter as `Display Name` then name `catalog`, enter your email and click
`Create App ID`.
- Click `Set Up` button of the `Facebook Login` card.
- Choose Web Plateform.
- Enter `http://13.59.39.163/` as site URL and ckick `Save` button.
- Click `Settings` under `Facebook Login`, and put `http://13.232.238.49` and
`http://2dd1763c602c62b6708517937492a65f-2102374274.ap-south-1.elb.amazonaws.com/` as the Valid OAuth redirect URIs, and click `Save Changes` button.
- Click `Dashboard` on left menu. You should see `API Version` and `App ID` for the `catalog` application.
- Replace the `appId` and `version`, respectively on lines 74 and 78 of the `templates/login.html`, with the correspoding `App ID` and `API Version`.
- Open `/var/www/catalog/catalog/fb_client_secrets.json` file and replace `app_id` and `app_secret`.
-->

### Install the virtual environment and dependencies

- While logged in as `grader`, install pip: `sudo apt-get install python3-pip`.
- Install the virtual environment: `sudo apt-get install python-virtualenv`
- Change to the `/var/www/catalog/catalog/` directory.
- Create the virtual environment: `sudo virtualenv -p python3 venv3`.
- Change the ownership to `grader` with: `sudo chown -R grader:grader venv3/`.
- Activate the new environment: `. venv3/bin/activate`.
- Install the following dependencies:
  ```
  pip install httplib2
  pip install requests
  pip install --upgrade oauth2client
  pip install sqlalchemy
  pip install flask
  sudo apt-get install libpq-dev
  pip install psycopg2
  ```

- Run `python3 __init__.py`

- Deactivate the virtual environment: `deactivate`.

**References**
- Flask documentation, [virtualenv](http://flask.pocoo.org/docs/0.12/installation/).
- [Create a Python 3 virtual environment](https://superuser.com/questions/1039369/create-a-python-3-virtual-environment).






### Set up and enable a virtual host

- Add the following line in `/etc/apache2/mods-enabled/wsgi.conf` file
to use Python 3.

  ```
  #WSGIPythonPath directory|directory-1:directory-2:...
  WSGIPythonPath /var/www/catalog/catalog/venv3/lib/python3.5/site-packages
  ```

- Create `/etc/apache2/sites-available/catalog.conf` and add the
following lines to configure the virtual host:

  ```
  <VirtualHost *:80>
	  ServerName 13.232.238.49
    ServerAlias 2dd1763c602c62b6708517937492a65f-2102374274.ap-south-1.elb.amazonaws.com/
	  WSGIScriptAlias / /var/www/catalog/catalog.wsgi
	  <Directory /var/www/catalog/catalog/>
	  	Order allow,deny
		  Allow from all
	  </Directory>
	  Alias /static /var/www/catalog/catalog/static
	  <Directory /var/www/catalog/catalog/static/>
		  Order allow,deny
		  Allow from all
	  </Directory>
	  ErrorLog ${APACHE_LOG_DIR}/error.log
	  LogLevel warn
	  CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>
  ```

- Enable virtual host: `sudo a2ensite catalog`. The following prompt will be returned:
  ```
  Enabling site catalog.
  To activate the new configuration, you need to run:
    service apache2 reload
  ```

- Reload Apache: `sudo service apache2 reload`.

**Resources**
- [Getting Flask to use Python3 (Apache/mod_wsgi)](https://stackoverflow.com/questions/30642894/getting-flask-to-use-python3-apache-mod-wsgi)
- [Run mod_wsgi with virtualenv or Python with version different that system default](https://stackoverflow.com/questions/27450998/run-mod-wsgi-with-virtualenv-or-python-with-version-different-that-system-defaul)


### Set up the Flask application

- Create `/var/www/catalog/catalog.wsgi` file add the following lines:

  ```
  activate_this = '/var/www/catalog/catalog/venv3/bin/activate_this.py'
  with open(activate_this) as file_:
      exec(file_.read(), dict(__file__=activate_this))

  #!/usr/bin/python
  import sys
  import logging
  logging.basicConfig(stream=sys.stderr)
  sys.path.insert(0, "/var/www/catalog/catalog/")
  sys.path.insert(1, "/var/www/catalog/")

  from catalog import app as application
  application.secret_key = "..."
  ```

- Restart Apache: `sudo service apache2 restart`.

**Resource**
- Flask documentation, [Working with Virtual Environments](http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi/#working-with-virtual-environments)


### Set up the database schema and populate the database

- Edit `/var/www/catalog/catalog/data.py`.
- Replace `lig.random_para(250)` by `lig.random_para(240)` on lines 86, 143, 191, 234 and 280.

- Add the these two lines at the beginning of the file.

  ```
  import sys
  sys.path.insert(0, "/var/www/catalog/catalog/venv3/lib/python3.5/site-packages")
  ```

- Add the following lines under `create_db()`.

  ```
  # Create database.
  create_db()

  # Delete all rows.
  session.query(Item).delete()
  session.query(Category).delete()
  session.query(User).delete()
  ```

- From the `/var/www/catalog/catalog/` directory,
activate the virtual environment: `. venv3/bin/activate`.
- Run: `python data.py`.
- Deactivate the virtual environment: `deactivate`.

### Disable the default Apache site

- Disable the default Apache site: `sudo a2dissite 000-default.conf`.
The following prompt will be returned:

  ```
  Site 000-default disabled.
  To activate the new configuration, you need to run:
    service apache2 reload
  ```

- Reload Apache: `sudo service apache2 reload`.

### Launch the Web Application

- Change the ownership of the project directories: `sudo chown -R www-data:www-data catalog/`.
- Restart Apache again: `sudo service apache2 restart`.
- Open your browser to your hostname




## Other Helpful Resources
- I got huge help from my Fellow Coursemate Akhil H THANK YOU Akhil
- Pilot test application test was made possible by [Sruti](https://github.com/SruthiV) a graduate
- Furthermore thanks to [boisalai](https://github.com/boisalai) for helping in PostgreSQL.

- DigitalOcean [How To Deploy a Flask Application on an Ubuntu VPS](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
- GitHub Repositories
  - [adityamehra/udacity-linux-server-configuration](https://github.com/adityamehra/udacity-linux-server-configuration)
  - [anumsh/Linux-Server-Configuration](https://github.com/anumsh/Linux-Server-Configuration)
  - [bencam/linux-server-configuration](https://github.com/bencam/linux-server-configuration)
  - [iliketomatoes/linux_server_configuration](https://github.com/iliketomatoes/linux_server_configuration)
