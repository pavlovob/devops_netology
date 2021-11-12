1. VirtualBox установлен  
2. Vagrant установлен  
3. Используемый терминал - Windows Terminal  
4. Создана рабочая директоря Vagrant. 
 - Инициализирован командой "vagrant init ubuntu/focal64". Дистрибутив Ubuntu 20.04LTS Focal Fossa
 - Команда "vagrant up" вне зависимости от дистрибутива не видела ни один box до тех пор пока в Vagrantfile не был добавлен параметр "config.vm.box_download_insecure = true", после чего все скачалось и запустилось.
