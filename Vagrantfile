Vagrant.require_version ">= 1.6.0"

# OSがWindowsか判定する
def windows?
  (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
end

required_plugins = %w(vagrant-ignition)
required_plugins << "vagrant-winnfsd" if windows? 

plugins_to_install = required_plugins.select { |plugin| not Vagrant.has_plugin? plugin }
unless plugins_to_install.empty?
  puts "Installing plugins: #{plugins_to_install.join(' ')}"
  if system "vagrant plugin install #{plugins_to_install.join(' ')}"
    exec "vagrant #{ARGV.join(' ')}"
  else
    abort "Installation of one or more plugins has failed. Aborting."
  end
end

# Nokogiriのインストールに必要なもの
BUNDLE_CONFIG_COMMAND = "bundle config --local build.nokogiri --use-system-libraries".freeze
# railsのインストール
BUNDLE_SETUP_COMMAND = "bundle install --path vendor/bundle".freeze
RAILS_NEW_COMMAND = "bundle exec rails new . -T -S --force --webpack=react --database=mysql".freeze

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.ssh.forward_agent = true
  config.vm.box = "coreos-stable"
  config.vm.box_url = "https://stable.release.core-os.net/amd64-usr/current/coreos_production_vagrant_virtualbox.json"

  config.vm.provider :virtualbox do |v|
    v.check_guest_additions = false
    v.functional_vboxsf     = false
    config.ignition.enabled = true
  end

  if Vagrant.has_plugin?("vagrant-vbguest") then
    config.vbguest.auto_update = false
  end

  config.vm.define vm_name = "core-01" do |config|
    config.vm.hostname = vm_name
    ip = "192.168.33.10"

    config.vm.network :private_network, ip: ip
    config.ignition.ip = ip
    config.vm.network "forwarded_port", guest: 3000, host: 8080, host_ip: "127.0.0.1", auto_correct: true
    config.vm.synced_folder "./share", "/home/core/share", nfs: true, mount_options: ['nolock,vers=3,udp']

    config.vm.provider :virtualbox do |vb|
      vb.gui = false
      vb.memory = 1024
      vb.cpus = 1
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", 100]
      config.ignition.config_obj = vb
    end

    # dockerイメージのダウンロード
    config.vm.provision :shell, inline: <<-SHELL
      docker pull ruby:2.5.3
      docker pull mysql:5.7
    SHELL

    # docker-composeのインストール
    config.vm.provision :shell, inline: <<-SHELL
      mkdir -p /opt/bin
      curl -L https://github.com/docker/compose/releases/download/1.12.0/docker-compose-`uname -s`-`uname -m` > /opt/bin/docker-compose
      chmod +x /opt/bin/docker-compose
    SHELL
  end
end