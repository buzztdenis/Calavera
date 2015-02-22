# this is branched to Lab 04 only. all machines removed except Manos.
#Berksfile tweak needed per https://github.com/berkshelf/vagrant-berkshelf/issues/237  **/.git

# dependencies:
  # manos => cerebro
  # manos => hombros
  # hombros => brazos
  # hombros => espina
  # hombros => cerebro
  # cara => espina
# resolved order: cerebro, brazos, espina, hombros, **manually setup artifactory**, manos, cara 

Vagrant.configure(2) do |config|
        if ARGV[1]=='base'
		config.vm.box = "opscode-ubuntu-14.04" # if you run base and repackage this will speed things up considerably	
	else
		config.vm.box = "opscode-ubuntu-14.04a"	# this box will not be on your machine to start
	end
  config.ssh.private_key_path = "~/.ssh/insecure_private_key"	
  config.berkshelf.enabled = true
	
	# how to boost capacity
      #config.vm.provider :virtualbox do |virtualbox|
			#virtualbox.customize ["modifyvm", :id, "--memory", "1024"]   # e.g. for Chef Server
			#virtualbox.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
		#end
 

        
###############################################################################
###################################    manos     ##############################
###############################################################################

	config.vm.define "manos" do | manos |
		manos.vm.host_name		="manos.calavera.biz"	
#		manos.vm.network 		"private_network", ip: "192.168.33.34"
#		manos.vm.network 		"forwarded_port", guest: 22, host: 2222, id: "ssh", disabled: true
#		manos.vm.network 		"forwarded_port", guest: 22, host: 2234, auto_correct: true
#		manos.vm.network 		"forwarded_port", guest: 80, host: 8034
#		manos.vm.network		"forwarded_port", guest: 8080, host: 8134
		manos.vm.synced_folder           ".", "/home/manos"
		manos.vm.synced_folder           "./shared", "/mnt/shared"                
		#manos.vm.provision 	    	:shell, path: "./shell/manos.sh"
		manos.vm.provision :chef_zero do |chef|
         chef.cookbooks_path = ["./cookbooks/"]
		   chef.add_recipe "shared::default"
         chef.add_recipe "git::default"
         chef.add_recipe "localAnt::default"
		  chef.add_recipe "java7::default"   # for some reason the Java recipe must be re-run to install Tomcat
		  chef.add_recipe "tomcat::default"
		  chef.add_recipe "shared::_junit"
		  chef.add_recipe "manos::default"
      end
	end
	
	# test: http://192.168.33.34:8080/MainServlet  # why port is not forwarding?
	# if cerebro is configured:
	# git add .
	# git commit -m "some message"
	# git push origin master

	
end