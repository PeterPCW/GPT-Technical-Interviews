# Explain how Chef works and how it is used for Configuration Management. Describe the architecture of a Chef deployment, including the Chef Server, Workstation, and Nodes. Provide an example of a Chef recipe that installs and configures Apache on a node.

Chef is an open-source configuration management tool used to manage the configuration of servers and automate repetitive infrastructure tasks. It is built around the concept of resources, which represent a piece of configuration that needs to be managed. Chef uses a declarative language to specify the desired state of each resource, and applies changes to the system to bring it into compliance with that desired state.

The architecture of a Chef deployment consists of three main components: the Chef Server, Workstation, and Nodes. The Chef Server is the central hub of the system, where all the configuration data and policies are stored. The Workstation is the machine where system administrators create and manage Chef code, such as cookbooks and recipes, and use the Chef tools to interact with the Chef Server. The Nodes are the machines that Chef manages, and each Node has a Chef Client installed, which communicates with the Chef Server to obtain the configuration information and apply it to the system.

Here is an example of a Chef recipe that installs and configures Apache on a node:

```ruby
# Cookbook Name:: apache
# Recipe:: default

package 'apache2' do
  action :install
end

service 'apache2' do
  supports :status => true, :restart => true, :reload => true
  action [ :enable, :start ]
end

template '/var/www/html/index.html' do
  source 'index.html.erb'
  mode '0644'
end
```

This recipe first installs the Apache package using the package resource, then configures the Apache service to start automatically and creates a template for the default index.html file. The template resource uses an ERB template to dynamically generate the HTML content for the index.html file.