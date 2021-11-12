VirtualBox установлен  
Vagrant установлен  
Используемый терминал - Windows Terminal  
Создана рабочая директоря Vagrant. 
 - Инициализирован командой "vagrant init ubuntu/focal64". Дистрибутив Ubuntu 20.04LTS Focal Fossa
 - Команда "vagrant up" вне зависимости от дистрибутива не видела ни один box до тех пор пока в Vagrantfile не была добавлен параметр "config.vm.box_download_insecure = true", после чего все скачалось и запустилось.
