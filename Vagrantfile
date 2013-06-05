Vagrant.configure('2') do |config|

  config.hostmanager.enabled           = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline   = true
  config.vm.provision :hostmanager
  config.librarian_chef.cheffile_dir   = 'chef'
  config.omnibus.chef_version = :latest

  PROVIDER         = :digital_ocean
  DEFAULT_REGION   = 'Amsterdam 1'
  CONFIGURATION    = { erlang1: { region: DEFAULT_REGION},
                       erlang2: { region: DEFAULT_REGION} }
  PRIVATE_KEY_PATH = '~/.ssh/id_rsa'

  CONFIGURATION.keys.each do |host_name|
    config.vm.define host_name do |host|
      host.vm.box = 'digital_ocean'
      host.vm.provider :digital_ocean do |provider|
        provider.region    = CONFIGURATION[host_name][:region]
        provider.client_id = ENV['DIGITAL_OCEAN_CLIENT_ID']
        provider.api_key   = ENV['DIGITAL_OCEAN_API_KEY']
      end
      host.vm.provision :chef_solo do |chef|
        chef.cookbooks_path = "chef/cookbooks"
      end
    end
  end

  config.ssh.private_key_path = PRIVATE_KEY_PATH
end
