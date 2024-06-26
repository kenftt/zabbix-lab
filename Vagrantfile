Vagrant.configure(2) do |config|

    # before you must install these plugins to speed up vagrant provisionning
    # vagrant plugin install vagrant-faster
    # vagrant plugin install vagrant-cachier

    config.cache.auto_detect = true
    # Set some variables
    etcHosts = ""
    zabbix = ""

    case ARGV[0]
    when "provision", "up"
        print "Do you want to install zabbix (yes/no) ?\n"
        zabbix = STDIN.gets.chomp
    end

    # some settings for common server (not for haproxy)
    common = <<-SHELL
        sudo apt update -y -qq 2>&1 >/dev/null
        sudo apt install -y -qq curl vim tree net-tools telnet git python3-pip sshpass 2>&1 >/dev/null
        sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/g' /etc/ssh/sshd_config
        sudo systemctl restart sshd
    SHELL

    config.vm.box = "bento/ubuntu-22.04"
    config.vm.box_url = "bento/ubuntu-22.04"

    # set servers list and their parameters
    NODES = [
        { :hostname => "zpg1", :ip => "192.168.56.100", :cpus => 4, :mem => 4096, :type => "postgres" },
        { :hostname => "zabbix1", :ip => "192.168.56.101", :cpus => 4, :mem => 4096, :type => "zabbix" },
        { :hostname => "z2", :ip => "192.168.56.102", :cpus => 2, :mem => 1024, :type => "node" }
    ]

    # define /etc/hosts for all servers
    NODES.each do |node|
        etcHosts += "echo '" + node[:ip] + "   " + node[:hostname] + "'>> /etc/hosts" + "\n"
    end #end NODES

    # run installation
    NODES.each do |node|
        config.vm.define node[:hostname] do |cfg|
            cfg.vm.hostname = node[:hostname]
            cfg.vm.network "private_network", ip: node[:ip]
            cfg.vm.provider "virtualbox" do |v|
                v.customize [ "modifyvm", :id, "--cpus", node[:cpus] ]
                v.customize [ "modifyvm", :id, "--memory", node[:mem] ]
                v.customize [ "modifyvm", :id, "--natdnshostresolver1", "on" ]
                v.customize [ "modifyvm", :id, "--natdnsproxy1", "on" ]
                v.customize [ "modifyvm", :id, "--name", node[:hostname] ]
                v.customize [ "modifyvm", :id, "--ioapic", "on" ]
                v.customize [ "modifyvm", :id, "--nictype1", "virtio" ]
                v.customize [ "modifyvm", :id, "--cableconnected1", "on" ]
            end #end provider

            # provisionning for all servers
            cfg.vm.provision :shell, :inline => etcHosts
            cfg.vm.provision :shell, :inline => common
            cfg.vm.provision :shell, :path => "install_aliases.sh"
            
            if zabbix == "yes"
                if node[:type] == "postgres"
                    cfg.vm.provision :shell, :path => "install_postgresql.sh"
                end
                if node[:type] == "zabbix"
                    cfg.vm.provision :shell, :path => "install_zabbix_server.sh"
                end
                if node[:type] == "node"
                    cfg.vm.provision :shell, :path => "install_zabbix_agent.sh"
                end
            end # end zabbix
        end # end config
    end # end nodes
end # end configure
