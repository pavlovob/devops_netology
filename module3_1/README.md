1. VirtualBox установлен  
2. Vagrant установлен  
3. Используемый терминал - Windows Terminal  
4. Создана рабочая директоря Vagrant. 
 - Инициализирован командой "vagrant init ubuntu/focal64". Дистрибутив Ubuntu 20.04LTS Focal Fossa  
 - Команда "vagrant up" вне зависимости от дистрибутива не видела ни один box до тех пор пока в Vagrantfile не был добавлен параметр "config.vm.box_download_insecure = true", после чего все скачалось и запустилось. По команде "vagrant ssh" успешно произведен вход в консоль (по умолчанию реквизиты для входа vagrant/vagrant)  
 - Команда 'vagrant suspend' успешно останалвивает вирт.машину с сохранением состояния, о чем есть подтверждение в GUI VirtualBox. Команда 'vagrant halt' выключила машину штатным методом ОС.  
5. С GUI ознакомился, по умолчанию вирт.машине выделены следующие ресурсы:  
![res](img/img3-1-1.PNG)
6. С докуменгтацией по vagrantfile ознакомлен. Для того, чтобы добавить в машину памяти или ресурсов процессора нужно добавить/изменить в vagrantfile секцию "config.vm.provider":  
config.vm.provider "virtualbox" do |v|  
  v.memory = 1024  
  v.cpus = 2  
end  
где memory - объем ОЗУ  
    cpus   - кол-во процессоров  
изменено на 2048/4:  
![res](img/img3-1-2.PNG)
