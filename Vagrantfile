# stripped down file for provisioning just one empty dev box. 

Vagrant.configure(2) do |config|
  config.vm.box = "opscode-ubuntu-14.04a"  # this box will not be on your machine to start
  config.ssh.private_key_path = "~/.ssh/insecure_private_key"	
  config.berkshelf.enabled = true

  ###############################################################################
  ###################################    manos40     ##############################
  ###############################################################################

# replace last 2 digits as appropriate

    config.vm.define "manos40" do | manos40 |
      manos40.vm.host_name            ="manos40.calavera.biz"
      manos40.vm.network              "private_network", ip: "192.168.33.40"   #END USER MUST UPDATE PER GUIDANCE
      manos40.vm.network              "forwarded_port", guest: 22, host: 2240, auto_correct: true
      manos40.vm.network              "forwarded_port", guest: 80, host: 8040
      manos40.vm.network              "forwarded_port", guest: 8080, host: 8140
      
      manos40.ssh.forward_agent        =true

      manos40.vm.synced_folder        ".",         "/home/manos40"
      manos40.vm.synced_folder        "./shared", "/mnt/shared"

      manos40.vm.provision :chef_zero do |chef|
        chef.cookbooks_path         = ["./cookbooks/"]
        chef.add_recipe             "git::default"
        chef.add_recipe             "localAnt::default"
        chef.add_recipe             "java7::default"   # for some reason the Java recipe must be re-run to install Tomcat
        chef.add_recipe             "tomcat::default"
        chef.add_recipe             "shared::_junit"
        # not running a manosXX recipe per se. user should manually clone hijo repo from espina
      end
    end

###############################################################################

end
