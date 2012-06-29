Vagrant::Config.run do |config|
    # This vagrant will be running on centos 6.2, 64bit with puppet provisioning
    config.vm.box = 'centos-62-64-puppet'
    config.vm.box_url = 'http://packages.vstone.eu/vagrant-boxes/centos/6.2/centos-6.2-64bit-puppet-vbox.4.1.12.box'

    # Use :gui for showing a display for easy debugging of vagrant
    config.vm.boot_mode = :gui

    config.vm.define :project do |project_config|
        project_config.vm.host_name = "www.project.dev"

        project_config.vm.network :hostonly, "33.33.33.10"

        # Pass custom arguments to VBoxManage before booting VM
        project_config.vm.customize [
            'modifyvm', :id, '--chipset', 'ich9', # solves kernel panic issue on some host machines
            '--uartmode1', 'file', 'C:\\base6-console.log' # uncomment to change log location on Windows
        ]

        # Pass installation procedure over to Puppet (see `support/puppet/manifests/project.pp`)
        project_config.vm.provision :puppet do |puppet|
            puppet.manifests_path = "support/puppet/manifests"
            puppet.module_path = "support/puppet/modules"
            puppet.manifest_file = "project.pp"
            puppet.options = [
                '--verbose',
#                '--debug',
            ]
        end
    end
end
