# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-18.04"
  config.vm.box_check_update = false
  config.vm.network :forwarded_port, guest: 80, host: 8080
  config.vm.network :forwarded_port, guest: 22, host: 12222, id: "ssh"
  #config.vm.network "public_network"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 2
  end

  config.vm.provision "shell", inline: <<-SHELL

    # Prepare Yocto environment.
    sudo apt-get update
    sudo apt-get install -y gawk wget git-core diffstat unzip texinfo gcc-multilib \
    build-essential chrpath socat cpio python python3 python3-pip python3-pexpect \
    xz-utils debianutils iputils-ping libsdl1.2-dev xterm

    # static code analisis tool
    sudo apt install -y splint nginx

  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    # Yocto build
    # Gatesgarth(Yocto 3.2)
    mkdir ~/yocto
    cd ~/yocto
    git clone git://git.yoctoproject.org/poky
    cd poky
    git checkout -b gatesgarth

    # add raspberrypi BSP
    cd ~/yocto/poky
    git clone git://git.yoctoproject.org/meta-raspberrypi
    cd meta-raspberrypi
    git checkout -b gatesgarth

    cd ~/yocto/poky
    source oe-init-build-env build
    bitbake-layers add-layer ../meta-raspberrypi/

    cd ~/yocto/poky
    git clone git://git.openembedded.org/meta-openembedded
    cd meta-openembedded
    git checkout -b gatesgarth
    cd ~/yocto/poky/build
    bitbake-layers add-layer ../meta-openembedded/meta-oe/
    bitbake-layers add-layer ../meta-openembedded/meta-python/
    bitbake-layers add-layer ../meta-openembedded/meta-multimedia/
    bitbake-layers add-layer ../meta-openembedded/meta-networking/

    # add test static analysis layer
    cd ~/yocto/poky
    git clone https://github.com/fnsfan29/meta-testsa.git
    bitbake-layers add-layer meta-testsa
    echo 'IMAGE_INSTALL_append = " helloworld"' >> build/conf/local.conf

    # build
    cd ~/yocto/poky/build
    bitbake core-image-base

  SHELL

end


