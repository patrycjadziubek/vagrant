Vagrant.configure('2') do |config|

  config.vm.hostname = 'localhost'
  config.vm.box = 'hashicorp/precise64'
  config.vm.box_check_update = false

  config.vm.synced_folder './', '/vagrant'

 config.vm.define 'jar' do |jar|
    jar.vm.network :forwarded_port, guest: 8080, host: 8080
    jar.vm.network :forwarded_port, guest: 80, host: 80

    jar.vm.provider :virtualbox do |vb|
      vb.name = 'dropwizard-jar'
    end

    jar.vm.provision :shell do |shell|
      shell.inline = <<-SHELL
	sudo apt-get update -y --fix-missing && sudo apt-get install python-software-properties -y
        sudo apt-get install -y software-properties-common
        sudo add-apt-repository ppa:openjdk-r/ppa -y
	sudo apt-get update -y
	sudo apt-get install openjdk-8-jdk -y
	sudo apt-get install maven -y
	sudo apt-get install git -y
	sudo apt-get autoremove -y
	sudo mkdir -p /app
	cd /app
        sudo git clone https://github.com/haroonzone/hello-dropwizard.git
	sudo apt-get install -y apache2 -y
        sudo a2enmod proxy ; sudo a2enmod proxy_http ; sudo a2enmod proxy_balancer ; sudo a2enmod proxy_balancer ; sudo service apache2 restart
	cd /app ; sudo git clone https://github.com/patrycjadziubek/vagrantxdocker.git
        sudo cp /app/vagrantxdocker/default /etc/apache2/sites-available/ ; sudo service apache2 reload
	cd /app/hello-dropwizard
	sudo mvn package
	sudo java -jar target/hello-dropwizard-1.0-SNAPSHOT.jar server example.yaml >/dev/null 2>&1 &
      SHELL
    end
  end

end
