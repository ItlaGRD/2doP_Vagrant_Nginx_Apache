Vagrant.configure("2") do |config|
  # Configuración para el servidor Apache 1
  config.vm.define "apache1" do |apache1|
    apache1.vm.box = "bento/ubuntu-20.04"
    apache1.vm.hostname = "apache1"
    apache1.vm.network "private_network", ip: "192.168.56.11"
    apache1.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    apache1.vm.synced_folder "./paginas_apache1", "/var/www/html", create: true
    apache1.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      echo "<h1>Servidor Apache 1</h1>" | sudo tee /var/www/html/index.html
      sudo systemctl restart apache2
    SHELL
  end

  # Configuración para el servidor Apache 2
  config.vm.define "apache2" do |apache2|
    apache2.vm.box = "bento/ubuntu-20.04"
    apache2.vm.hostname = "apache2"
    apache2.vm.network "private_network", ip: "192.168.56.12"
    apache2.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    apache2.vm.synced_folder "./paginas_apache2", "/var/www/html", create: true
    apache2.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      echo "<h1>Servidor Apache 2</h1>" | sudo tee /var/www/html/index.html
      sudo systemctl restart apache2
    SHELL
  end

  # Configuración para el servidor Nginx (Load Balancer)
  config.vm.define "nginx" do |nginx|
    nginx.vm.box = "bento/ubuntu-20.04"
    nginx.vm.hostname = "nginx"
    nginx.vm.network "private_network", ip: "192.168.56.13"
    nginx.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    nginx.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y nginx
      sudo tee /etc/nginx/conf.d/load_balancer.conf > /dev/null <<EOL
upstream backend {
    server 192.168.56.11;
    server 192.168.56.12;
}

server {
    listen 80;
    location / {
        proxy_pass http://backend;
    }
}
EOL
      sudo systemctl restart nginx
    SHELL
  end
end

