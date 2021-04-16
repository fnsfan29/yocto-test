# yocto-test
test project for yocto renrakukai

## Description
- host: bento/ubuntu-18.04
- yocto: Gatesgarth(Yocto 3.2) add raspberrypi BSP

## Requirement
- [Oracle VM VirtualBox](https://www.virtualbox.org/)
- [Vagrant by HashiCorp](https://www.vagrantup.com/)

## Usage
```sh
$ git clone https://github.com/fnsfan29/yocto-test.git
$ cd yocto-test
$ vagrant up
$ vagrant ssh
```

## QEMU
```sh
$ cd ~/yocto/poky
$ source oe-init-build-env build
$ runqemu qemux86-64 nographic
```

## Static code analysis
[Splint Home Page](http://splint.org/)
