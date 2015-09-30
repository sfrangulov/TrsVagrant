Vagrant.configure(2) do |config|
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "forwarded_port", guest: 3000, host: 3000
  config.vm.network "forwarded_port", guest: 3001, host: 3001
  config.vm.network "forwarded_port", guest: 9000, host: 9000
  config.vm.network "forwarded_port", guest: 35729, host: 35729
  config.vm.network "forwarded_port", guest: 5858, host: 5858
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.synced_folder "src/", "/home/vagrant/nfs", create: true, nfs: true
  config.bindfs.bind_folder "/home/vagrant/nfs", "/home/vagrant/src"
  config.vm.provider "virtualbox" do |vb|
     vb.memory = "2048"
  end
  config.vm.provision "shell", inline: <<-SHELL
      export LANGUAGE=en_US.UTF-8
      export LANG=en_US.UTF-8
      export LC_ALL=en_US.UTF-8
      locale-gen en_US.UTF-8
      dpkg-reconfigure locales
      sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
      echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list
      curl -sL https://deb.nodesource.com/setup_0.12 | sudo bash -
      sudo apt-get install -y build-essential gcc make libfontconfig nodejs git libkrb5-dev mongodb-org curl libpng-dev
      sudo apt-get autoremove -y
      sudo npm install -g yo gulp grunt-cli bower db-migrate
      sudo npm install -g generator-karma generator-angular generator-gulp-angular generator-angular-fullstack
      sudo npm cache -g clean
      npm cache clean
      #sudo rm -rf ~/.npm /vagrant/node_modules
      sudo cp /vagrant/mongod.conf /etc
      mkdir -p /home/vagrant/mongodb/data
      mkdir -p /home/vagrant/mongodb/log
      mkdir -p /home/vagrant/mongodb/config
      sudo chown -R mongodb:mongodb /home/vagrant/mongodb
      sudo service mongod stop
      sudo service mongod start
      sudo gem install sass
  SHELL
end
