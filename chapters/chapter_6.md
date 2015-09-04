

#Maintenance cycle 




- General configuration structure for the chef-repo:
    * chef-repo/environments/banana.rb
    * chef-repo/environments/potato.rb
    * chef-repo/environments/kiwi.rb
    * chef-repo/data_bags/banana.rb
    * chef-repo/data_bags/potato.rb
    * chef-repo/data_bags/kiwi.rb: 
    * chef-repo/roles/base.rb
    * chef-repo/roles/web.rb
    * chef-repo/roles/db.rb

- Banana cookbook structure:
    * chef-repo/cookbooks/banana/templates/default/*.erb
    * chef-repo/cookbooks/banana/attributes/default.rb

- Potato cookbook structure:
    * chef-repo/cookbooks/potato/templates/default/*.erb
    * chef-repo/cookbooks/potato/attributes/default.rb

-Kiwi cookbook structure:
    * chef-repo/cookbooks/kiwi/templates/default/*.erb
    * chef-repo/cookbooks/kiwi/attributes/default.rb

A node belongs to an environment in which case, will override the default configuration per the corresponding one.



Override app attributes for kiwi (Non sensitive info)
Override app attributes for kiwi (Sensitive info)
Define the recipes for the xxx role

Default configuration templates for kiwi
Default configuration values according the templates for kiwi





