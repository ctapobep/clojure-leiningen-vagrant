Vagrant.configure("2") do |config|
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.auto_detect = true
  end
  config.ssh.forward_agent = true
  #you can install x11 in external custom bash script if needed
  config.ssh.forward_x11 = true
end


Vagrant::Config.run do |config|
  config.vm.box = "precise32"
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"
  config.vm.forward_port 3000, 3000

  config.vm.provision :chef_solo do |chef|
  #configuring soft that's going to be installed
    chef.json.merge!({
      :java => {
        "install_flavor" => "oracle",
        :oracle => {"accept_oracle_download_terms" => true},
        :jdk_version => "7"
       },
       :postgresql => {
         :password => {
           'postgres' => 'postgres'
         },
         :config => {
           'ssl' => false
         },
         :version => "9.3" 
       }
    })
    #updating package caches to install fresh soft
    chef.add_recipe("java")
    chef.add_recipe("apt")
    chef.add_recipe("openssl::default")
    chef.add_recipe("git")
    chef.add_recipe("postgresql")
    chef.add_recipe("postgresql::server")
    chef.add_recipe("leiningen")
  end

  external_script_name = "custom-shell-scripts/custom.sh"
  if File.exist?(external_script_name)
    config.vm.provision "shell", path: external_script_name
  end
end

