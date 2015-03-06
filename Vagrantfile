# do NOT just "vagrant up" this whole cluster. not recommended.
# currently, need to do startup.sh and then vagrant up the machines one by on in this order
#  cerebro, brazos, espina, hombros, **manually setup artifactory**, manos, cara

#Berksfile tweak needed per https://github.com/berkshelf/vagrant-berkshelf/issues/237  **/.git

# dependencies:
  # manos => cerebro
  # manos => hombros
  # hombros => brazos
  # hombros => espina
  # hombros => cerebro
  # cara => espina

Vagrant.configure(2) do |config|
  if ARGV[1]=='base'
    config.vm.box = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-14.04_chef-provisionerless.box"
  else
    config.vm.box = "opscode-ubuntu-14.04a"  # this box will not be on your machine to start
  end
  config.ssh.private_key_path = "~/.ssh/insecure_private_key"	
  config.berkshelf.enabled = true

  # how to boost capacity
      #config.vm.provider :virtualbox do |virtualbox|
      #virtualbox.customize ["modifyvm", :id, "--memory", "1024"]   # e.g. for Chef Server
      #virtualbox.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
    #end


###############################################################################
###################################    manos40     ##############################
###############################################################################

  config.vm.define "manos40" do | manos40 |
    manos40.vm.host_name            ="manos40.calavera.biz"
    manos40.vm.network              "private_network", ip: "192.168.33.40"
    manos40.vm.network              "forwarded_port", guest: 22, host: 2240, auto_correct: true
    manos40.vm.network              "forwarded_port", guest: 80, host: 8040
    manos40.vm.network              "forwarded_port", guest: 8080, host: 8140
    
    manos40.ssh.forward_agent        =true

    manos40.vm.synced_folder        ".",         "/home/manos40"
    manos40.vm.synced_folder        "./shared", "/mnt/shared"
    manos40.vm.synced_folder        "/var/SEIS660/public", "/mnt/public"  # course specific
    #manos40.vm.provision         :shell, path: "./shell/manos40.sh"

    manos40.vm.provision :chef_zero do |chef|
      chef.cookbooks_path         = ["./cookbooks/"]
      chef.add_recipe             "git::default"
      chef.add_recipe             "localAnt::default"
      chef.add_recipe             "java7::default"   # for some reason the Java recipe must be re-run to install Tomcat
      chef.add_recipe             "tomcat::default"
      chef.add_recipe             "shared::_junit"
    end

  end

  # test: http://192.168.33.34:8080/MainServlet
  # if cerebro is configured:
  # git add .
  # git commit -m "some message"
  # git push origin master




###############################################################################

end
