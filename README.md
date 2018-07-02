btxcore-deb
===========
Packaging system to deploy bitcore Block Explorer
**The current configuration of this repository creates deb packages suited to run on ubuntu, behind Cloudflare(snakeoil ssl only), and are customized for the domain https://bitcore.network**

Using the build environment 
------------------
````
$sudo apt-get update
$sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
$curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$sudo apt-get update
$sudo apt-get install -y docker-ce git make
$git clone https://github.com/BTXexplorer/btxcore-deb
$cd btxcore-deb/btx
$sudo make
````
DEB file will appear in btxcore-deb/btx/

Deploying a btxcore Deb file
------------------------------------
Download your deb to the home directory of your ubuntu instance
````
$sudo apt-get install -y apt-transport-https curl && sudo dpkg -i btxcore_<version>_amd64.deb
##the above will show errors, running the next command resolves those errors##
$sudo apt-get update && sudo apt-get -f install -y
$cd /etc/nginx/sites-enabled
$sudo ln -s ../sites-available/nginx-btxcore .
$sudo rm default
$sudo rm ../sites-available/default
$sudo systemctl enable mongod.service
$sudo service mongod start
$sudo service btxcore restart
$sudo printf "[Service]\nExecStartPost=/bin/sleep 0.1\n" | sudo tee /etc/systemd/system/nginx.service.d/override.conf
$sudo systemctl daemon-reload
$sudo service nginx restart
````
at this point the btxcore service and nginx will automatically launch and run even after reboot

Optional: add a redirect from www.example.com to example.com
----
````$sudo nano /etc/nginx/conf.d/redirect.conf````
and edit the file with:
````
server {
    server_name www.example.com;
    return 301 $scheme://example.com$request_uri;
}
````

Helpful commands to manage your Deb based install:
----
````
$sudo service btxcore start
$sudo service btxcore stop
$sudo service btxcore restart
$sudo service btxcore status
$journalctl -u btxcore

$sudo service nginx start
$sudo service nginx stop
$sudo service nginx restart
$sudo service nginx status
journalctl -u nginx
````
Undeploying a btxcore Deb file
----
````
$sudo apt-get install -y aptitude
$sudo aptitude purge btxcore
````
Updating btxcore version
---------------------------------
* Change version in git checkout in btx/btxcore/Makefile
* Create a new entry in btx/btxcore/debian/changelog by using 'dch -i'
* Then build as usual
