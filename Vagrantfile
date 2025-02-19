Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.boot_timeout = 600


  # Количество требуемых машин
  SERVERS = 3
  # Имя сетевого интерфейса для организации моста
  BRIDGE = "TP-Link Wireless USB Adapter"

   def create_host(config, hostname, ip, port)
    config.vm.define hostname do |host|
      host.vm.network "private_network", ip: ip
      host.vm.network "public_network", bridge: BRIDGE
      host.vm.hostname = hostname
      host.vm.provision "shell", inline: "apt-get update && apt-get install -y python-minimal"
      
      # Уникальный порт для каждого сервера
      host.vm.network "forwarded_port", guest: 22, host: port
      
      yield host if block_given?
    end
  end

  # Перебираем все серверы и создаём для каждого свой уникальный порт
  (1..SERVERS).each do |machine_id|
    port = 2200 + machine_id  
    create_host(config, "srv#{machine_id}", "192.168.56.#{200 + machine_id}", port)
  end

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"  
    vb.cpus = 2         
  end

end
