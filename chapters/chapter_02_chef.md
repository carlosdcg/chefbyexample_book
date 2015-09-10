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






##Install one Workstation


# GEt CHef DK
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


DOne!!!!