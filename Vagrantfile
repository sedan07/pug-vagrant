require 'yaml'

Vagrant.require_version ">= 1.5"

# Load in a the local config file
dir = File.dirname(File.expand_path(__FILE__))
configFile = "#{dir}/config.yaml"
if File.file?(configFile) == false then
  noConfigMsg = <<MSG

  config.yaml does not exist... Did you forget to copy config.yaml.dist and customise the values?

  Hint: You should probably try reading the README.md file

MSG
  abort(noConfigMsg)
end

configValues = YAML.load_file(configFile)
data = configValues['vagrant-config']

ENV['VAGRANT_NO_PARALLEL'] = '1'
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'

Vagrant.configure("2") do |config|

config.vm.define "pug-puppet-master" do |puppetmaster|
  puppetmaster.vm.synced_folder "#{data['puppet_repo_dir']}", "/etc/puppet/environments/production"
  puppetmaster.vm.provider "docker" do |d|
    d.image = "sedan07/pug-puppet-master"
    d.name = "pug-puppet-master"
    d.remains_running = true
    d.ports = [
  "8140:8140"
    ]
  end
end

config.vm.define "pug-web-https" do |https|
  https.vm.provider "docker" do |d|
    d.image = "sedan07/pug-web-https"
    d.name = "pug-web-https"
    d.link("pug-puppet-master:puppet")
    d.remains_running = true
    d.ports = [
  "443:443"
    ]
  end
end

config.vm.define "pug-web-http" do |http|
  http.vm.provider "docker" do |d|
    d.image = "sedan07/pug-web-http"
    d.name = "pug-web-http"
    d.link("pug-puppet-master:puppet")
    d.remains_running = true
    d.ports = [
  "80:80"
    ]
  end
end
end

