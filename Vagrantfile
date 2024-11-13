Vagrant.configure("2") do |config|
  # Web Server Configuration
  config.vm.define "web" do |web|
    web.vm.box = "ubuntu/bionic64"  # Ubuntu 18.04 LTS
    web.vm.hostname = "web.local"
    web.vm.network "private_network", ip: "192.168.56.10"
    web.vm.provider "virtualbox" do |vb|
      vb.name = "WebServer"
      vb.memory = "512"  # Allocate less memory initially
      vb.cpus = 1
    end
    web.vm.provision "shell", inline: <<-SHELL

 yes | sudo apt-get install -y ufw
      echo "y" | sudo ufw allow 80/tcp   # Allow HTTP traffic
      echo "y" | sudo ufw allow 443/tcp  # Allow HTTPS traffic 
      echo "y" | sudo ufw allow 22/tcp  # Allow SSH access
      echo "y" | sudo ufw enable
  sudo ufw --force enable
      sudo systemctl enable apache2
      sudo systemctl start apache2

 sudo apt-get update
  sudo apt-get install -y rsyslog
  sudo systemctl enable rsyslog
  sudo systemctl start rsyslog

    SHELL
  end

  # Database Server Configuration
  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/bionic64"
    db.vm.hostname = "db.local"
    db.vm.network "private_network", ip: "192.168.56.11"
    db.vm.provider "virtualbox" do |vb|
      vb.name = "DatabaseServer"
      vb.memory = "512"  # Allocate less memory initially
      vb.cpus = 1
    end
    db.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update -y
      sudo apt-get install -y mysql-server
	 yes | sudo apt-get install -y ufw	
      yes | sudo ufw allow from 192.168.56.10 to any port 3306
      yes | sudo ufw allow 22/tcp  # Allow SSH access
      yes | sudo ufw --force enable

  sudo ufw --force enable
      sudo systemctl enable mysql
      sudo systemctl start MySQL

sudo apt-get update
  sudo apt-get install -y rsyslog
  sudo systemctl enable rsyslog
  sudo systemctl start rsyslog

 
# Adding environment variables to .bashrc (user-specific)
  echo 'export DB_USER="my_db_user"' >> ~/.bashrc
  echo 'export DB_PASS="my_secure_password"' >> ~/.bashrc

  # Adding environment variables to a system-wide profile file
  echo 'export DB_USER="my_db_user"' | sudo tee /etc/profile.d/db_env.sh
  echo 'export DB_PASS="my_secure_password"' | sudo tee -a /etc/profile.d/db_env.sh
  sudo chmod 600 /etc/profile.d/db_env.sh  # Set permissions to 600 for security
  sudo chown root:root /etc/profile.d/db_env.sh  # Set ownership to root
  echo "Environment variables added successfully."
    SHELL
  end
end
