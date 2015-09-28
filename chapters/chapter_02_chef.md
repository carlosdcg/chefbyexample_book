# Chef


## Chef components


## Knife


## OHAI


## Atributes


## Databags


## Environments


## LIghtweight resources and providers


## Cookbooks


## Recipes

##Install CHEF


The first step is to check the server host name.

\begin{codelisting}
\label{code:hostname}
\codecaption{}
```bash
root@chef01:~# hostname -f
```
\end{codelisting}


Now we need to edit the host name configuration file in order to add the FQDN of the server.
\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# sudo nano /etc/hosts
```
\end{codelisting}

Your configuration file should be something similar to this one.

![02](images/figures/02_host_name.png)



Update the packages list and update the server.

\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# sudo aptitude update
root@chef01:~# sudo aptitude upgrade
```
\end{codelisting}

Download the latest version of the chef server to the root folder of the current user.

\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# wget https://web-dl.packagecloud.io/chef/stable/packages/ubuntu/trusty/chef-server-core_12.2.0-1_amd64.deb
```
\end{codelisting}


Install the Chef Server.

\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# sudo dpkg -i chef-server-core_*.deb
root@chef01:~# sudo chef-server-ctl reconfigure
```
\end{codelisting}

![06](images/figures/06_install_chef_api.PNG)

This should be the result....



Install additional modules.



(Premium freatures up to 25 nodes..)
\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# chef-server-ctl install opscode-manage;
root@chef01:~# chef-server-ctl install opscode-push-jobs-server;
root@chef01:~# chef-server-ctl install opscode-reporting;
root@chef01:~# opscode-manage-ctl reconfigure;
root@chef01:~# opscode-push-jobs-server-ctl reconfigure;
root@chef01:~# opscode-reporting-ctl reconfigure; 
root@chef01:~# chef-server-ctl reconfigure;

```
\end{codelisting}


We need to create the first user (Admin user)

\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# chef-server-ctl user-create admin the administrator the_good@chefbyexample.com 4dm1n1str4t0r -f admin.pem
```
\end{codelisting}


WE need to create the first organization and add the first created user to it.

REmember to change:

 * Your organization name name.
 * The display name for the organization.
 * The name of the validator key.

\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# sudo chef-server-ctl org-create chefbyexample "ChefByExample.com" --association_user admin -f chefbyexample-validator.pem
```
\end{codelisting}


![12](images/figures/12_install_chef_finished.PNG)

Now the Chef SErver is fully operative, now we need to add the Workstation.






##Install the Workstation


\begin{codelisting}
\label{code:}
\codecaption{}
```bash

sudo wget https://opscode-omnibus-packages.s3.amazonaws.com/ubuntu/12.04/x86_64/chefdk_0.7.0-1_amd64.deb

sudo dpkg -i chefdk_*.deb

sudo chef generate repo chef-repo

mkdir ~/chef-repo/.chef
scp root@chef01.chefbyexample.com:/root/admin.pem ~/chef-repo/.chef
scp root@chef01.chefbyexample.com:/root/chefbyexample-validator.pem ~/chef-repo/.chef

nano ~/chef-repo/.chef/knife.rb


current_dir = File.dirname(__FILE__)
log_level                :info
log_location             STDOUT
node_name                "admin"
client_key               "#{current_dir}/admin.pem"
validation_client_name   "chefbyexample-validator"
validation_key           "#{current_dir}/chefbyexample-validator.pem"
chef_server_url          "https://chef01.chefbyexample.com/organizations/chefbyexample"
syntax_check_cache_path  "#{ENV['HOME']}/.chef/syntaxcache"
cookbook_path            ["#{current_dir}/../cookbooks"]


knife ssl fetch

Bootstraping the first node

knife bootstrap node01.chefbyexample.com -N node01


```
\end{codelisting}







## Installing the Open Source Web Interface ##

### Requirements installation ###
Installation steps, just run:
\begin{codelisting}
\label{code:}
\codecaption{}
```bash
aptitude install git rubygems1.9.1 ruby1.9.1-dev build-essential;
mkdir -p /var/www/; cd /var/www/; git clone https://github.com/carlosdcg/chefbyexample_webui; cd chefbyexample_webui;
gem install bundler;
bundle install;
```
\end{codelisting}


### Configure the default paramenters ###
Configure the web app in /var/www/chefbyexample_webui/config/application.rb

\begin{codelisting}
\label{code:}
\codecaption{}
```ruby
config.chef_server_url = "http://127.0.0.1"
config.rest_client_name = "pivotal"
config.rest_client_key = "/etc/opscode/pivotal.pem"
config.admin_user_name =  "admin"
config.admin_default_password = "4dm1n1str4t0r"
config.rest_client_custom_http_headers = {}
#This app only supports one organization, like the Open Source Chef Server 11
config.default_organization = "organizations/chefbyexample/"
```
\end{codelisting}

### Use the interface on demand or install it as a service ###
Once the Web UI is installed, from /var/www/chefbyexample_webui run:

To test in the default port 9292:
\begin{codelisting}
\label{code:}
\codecaption{}
```bash
rackup config.ru
```
\end{codelisting}

To run as a daemon in another port to test:
\begin{codelisting}
\label{code:}
\codecaption{}
```bash
rackup config.ru -D -p 1234
```
\end{codelisting}

Once you have tested it, to create the init scripts and install the run levels---
\begin{codelisting}
\label{code:}
\codecaption{}
```bash
#TO remove the script from the default run-levels
#sudo update-rc.d -f chefbyexample_webui remove
sudo chmod 755 /var/www/chefbyexample_webui/init/chefbyexample_webui.sh
ln -s /var/www/chefbyexample_webui/init/chefbyexample_webui.sh /etc/init.d/chefbyexample_webui
sudo chmod 755 /etc/init.d/chefbyexample_webui
sudo chown root:root /etc/init.d/chefbyexample_webui
sudo update-rc.d chefbyexample_webui defaults
```
\end{codelisting}






