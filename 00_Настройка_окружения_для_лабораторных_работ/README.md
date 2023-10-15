# Настройка окружения для лабораторных работ

**Для базового знакомства с командной строкой linux** можно использовать эмуляторы терминала в веб-браузере, например:  
https://webvm.io/  
https://bellard.org/jslinux/ (есть возможность выбрать несколько дистрибутивов, нам подойдет: Fedora 33 Console)  

Для полноценных лабораторных работ используется связка Virtualbox + Vagrant и git. Окружения для лаб может быть как в различных ОС: windows и linux.

**Ссылки на необходимый софт**
* [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
* [Vagrant](https://www.vagrantup.com/downloads) - [Vagrant - Зеркало hashicorp от Yandex](https://hashicorp-releases.yandexcloud.net/vagrant/)
* [Git](https://git-scm.com/download/)

**Быстрый старт**
* [Vagrant](https://learn.hashicorp.com/collections/vagrant/getting-started)
* [Git](https://githowto.com/ru) 

#### Зеркало vagrant образов
* [Зеркало vagrant образов](https://vagrant.comcloud.xyz/boxes/search)

! чтобы не использовать локальные образы, можно в Vagrantfile указывать ссылку на образ через зеркало.  
Пример: vm.box_url = https://vagrant.comcloud.xyz/ubuntu/boxes/jammy64  

 
#### Использование локального образа:
* [Скачать box centos/7](https://disk.yandex.ru/d/1s0pATFHjFWHdA)
* [Скачать box centos/8](https://disk.yandex.ru/d/Pgy_NwE-APPE3A)

**Example**
```bash
vagrant box add centos/8 file:///d:/path/to/centos_8.box
```

! чтобы не использовать локальные образы, можно в Vagrantfile указывать ссылку на образ через зеркало.  
Пример: vm.box_url = https://vagrant.comcloud.xyz/ubuntu/boxes/jammy64  


#### Типовые ошибки (могут быть на windows)
Ошибка с кодировкой:
1. Установите переменную окружения VAGRANT_HOME , например 'c:\HashiCorp';
через bash git установить переменную VAGRANT_HOME:  
```bash
$ export VAGRANT_HOME=/d/Vagrant
```
2. В файле C:\HashiCorp\Vagrant\embedded\gems\2.2.3\gems\vagrant-2.2.3\bin\vagrant после #!/usr/bin/env добавить две строки:
Encoding.default_external = Encoding.find('Windows-1251')
Encoding.default_internal = Encoding.find('Windows-1251')  
