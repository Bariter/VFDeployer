# To support box versioning, set the min vagrant version.
Vagrant.require_version ">= 1.5.0"

ENV['AWS_PROFILE'] = 'vagrant-s3auth'

Vagrant.configure(2) do |config|
  # Non-provider specific configuration.
  config.vm.box = "bariter/deployer"
  config.vm.box_check_update = true
  config.vm.box_url = "https://s3.amazonaws.com/bariter-virtualbox/private_image_vb_deployer.json"
  config.vm.hostname = "deployer"
  config.vm.post_up_message = "Let's freak out!"
  config.vm.box_download_checksum_type = "sha1"
  config.vm.box_download_checksum = "d6ccaa87b28649c83a474c1abd834c789fd27a23"

  config.vm.provider :virtualbox do |v|
    v.name = "vb-deployer"
    config.vm.network :private_network, ip: "192.168.33.10",
      virtualbox__intnet: true
    v.gui = false
  end

  # AWS specific configuration. "override" is used 
  # to override the non-provider specific configurations.
  config.vm.provider :aws do |aws, override|
  end

  unless Vagrant.has_plugin?('vagrant-s3auth')
    # Attempt to install ourself. Bail out on failure so we don't get stuck in an
    # infinite loop.
    system('vagrant plugin install vagrant-s3auth') || exit!

    # Relaunch Vagrant so the plugin is detected. Exit with the same status code.
    exit system('vagrant' *ARGV)
  end
end
