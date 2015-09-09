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


![01](images/figures/01_host_name.png)

\begin{codelisting}
\label{code:hostname}
\codecaption{}
```bash
root@chef01:~# hostname -f
```
\end{codelisting}


![02](images/figures/02_host_name.png)

\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# sudo nano /etc/hosts
```
\end{codelisting}


![03](images/figures/03_install_chef.PNG)

\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# wget https://web-dl.packagecloud.io/chef/stable/packages/ubuntu/trusty/chef-server-core_12.2.0-1_amd64.deb
```
\end{codelisting}


![04](images/figures/04_install_chef.PNG)

\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# sudo dpkg -i chef-server-core_*.deb
```
\end{codelisting}


![05](images/figures/05_install_chef_config.PNG)

\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# sudo chef-server-ctl reconfigure
```
\end{codelisting}


![06](images/figures/06_install_chef_api.PNG)

Web interface


![07](images/figures/07_install_chef_web_ui.PNG)

(Premium freature up to 25 nodes..)
\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# chef-server-ctl install opscode-manage; chef-server-ctl reconfigure; opscode-manage-ctl reconfigure
```
\end{codelisting}


![08](images/figures/08_install_chef_push_jobs.PNG)

\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# chef-server-ctl install opscode-push-jobs-server; chef-server-ctl reconfigure; opscode-push-jobs-server-ctl reconfigure;
```
\end{codelisting}


![09](images/figures/09_install_chef_reporting.PNG)

(Premium freature up to 25 nodes..)
\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# chef-server-ctl install opscode-reporting; chef-server-ctl reconfigure; opscode-reporting-ctl reconfigure; 
```
\end{codelisting}

![10](images/figures/10_install_chef_add_user.PNG)

\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# chef-server-ctl user-create admin the administrator the_good@chefbyexample.com 4dm1n1str4t0r -f admin.pem
```
\end{codelisting}


![11](images/figures/11_install_chef_add_org.PNG)

\begin{codelisting}
\label{code:}
\codecaption{}
```bash
root@chef01:~# sudo chef-server-ctl org-create chefbyexample "ChefByExample.com" --association_user admin -f chefbyexample-validator.pem
```
\end{codelisting}


![12](images/figures/12_install_chef_finished.PNG)

Chef Server installed..

##Install old and rusty web interface

###Get the webui source
aptitude install git

mkdir -p /var/www/

cd /var/www/

git clone https://github.com/carlosdcg/chef-server-webui

cd chef-server-webui

###Install packages

aptitude install rubygems1.9.1 ruby1.9.1-dev build-essential

gem install bundler

bundle install




update in /var/www/chef-server-webui/config/application.rb



    config.chef_server_url = "http://127.0.0.1:8000"
    config.rest_client_name = "chef-webui"
    config.rest_client_key = "/etc/chef-server/webui_priv.pem"
    config.admin_user_name =  "admin"
    config.admin_default_password = "p@ssw0rd1"
    config.rest_client_custom_http_headers = {}

to

    config.chef_server_url = "https://127.0.0.1:443"
    config.rest_client_name = "chef-webui"
    config.rest_client_key = "/etc/opscode/webui_priv.pem"
    config.admin_user_name =  "admin"
    config.admin_default_password = "4dm1n1str4t0r"
    config.rest_client_custom_http_headers = {}


Change the default attr in /opt/opscode/embedded/cookbooks/private-chef/attributes/default.rb

default['private_chef']['opscode-webui']['enable'] = false
to
default['private_chef']['opscode-webui']['enable'] = true


default['private_chef']['default_orgname'] = nil
to
default['private_chef']['default_orgname'] = chefbyexample




Then run
chef-server-ctl reconfigure













##Install one Workstation
