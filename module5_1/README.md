# Домашнее задание к занятию "5.1. Введение в виртуализацию. Типы и функции гипервизоров. Обзор рынка вендоров и областей применения."

## Задача 1
- Полная (аппаратная виртуализация) характеризуется непосредственной установкой гипервизора на железо (гипервизор по сути сам является ОС) 
- Паравиртуализация от полной (аппаратной) отлличается тем, что гипервизор работает под управлением одной из ОС устанавливаемых на железо.
- Виртуализация на основе ОС основана на разделении адресного пространства процессов в гостевых системах, разделяя ресурсы ядра хостовой ОС.  
### Ответ на вопрос по доработке задания:
Разница при работе с ядром гостевой ОС гипервизоров полной и паравиртуалилизации состоит в том, что для ускорения некоторых операций ввода-вывода между физическими ресурсами и эмулируемыми паравиртуальные гипервизоры используют дополнительный набор драйверов устанавливаемых дополнительно (VMware Tools, VirtualBox Guest additions CD и т.д.)
## Задача 2
- Для высоконагруженных БД скорее всего лучше использовать физические сервера или виртуализацию уровня ОС для исключения задержек в работе с дисковой подсистемой которая есть в паравиртуализации  
- Для web приложений можно использовать паравиртуализацию т.к. на железе их размещать расточительно, а уровень ОС может оказаться ненадежным из-за общего ядра ОС для процессов.
- Windows системы для использования бухгалтерским отделом я бы разместил на железе (терминальный сервер) или край на паравиртуализации для удобства управления
- Системы, выполняющие высокопроизводительные расчеты на GPU - только "железо" для оптимальной работы с GPU
## Задача 3
Сценарии:
1. 100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.
- Для данного сценария я бы выбрал полную аппаратную виртуализацию известного брэнда. Данный выбор обусловлен тем, что ВМ много, ими необходимо эффективно управлять, делать резервные копии, скорее всего не обойдется без миграций и перераспределения нагрузки. Все это есть в утилитах той же VMWare.
3. Требуется наиболее производительное бесплатное open source решение для виртуализации небольшой (20-30 серверов) инфраструктуры на базе Linux и Windows виртуальных машин.
- В этом сценарии я бы использовал паравиртуализацию. Серверов немного, ими тоже нужно управлять, есть OpenSource решения (VirtualBox например, управлять машинами которого можно из Vagrant - хоть какая то автоматизация)
5. Необходимо бесплатное, максимально совместимое и производительное решение для виртуализации Windows инфраструктуры.
- В данном сценарии, учитывая главный аргумент - бесплатность, либо паравиртуализация VirtualBox либо полная аппаратная Citrix XenServer. Думаю использование последнего предпочтительнее
7. Необходимо рабочее окружение для тестирования программного продукта на нескольких дистрибутивах Linux.
- Здесь, думаю, подойдет паравиртуализация VirtualBox (через Vagrant для достижения стабильных результатов при переустановке систем) или нативная вируализация уровня ОС Linux
(в случае допустимости одинакового ядра для всех экземпляров ВМ).
## Задача 4
Опишите возможные проблемы и недостатки гетерогенной среды виртуализации (использования нескольких систем управления виртуализацией одновременно) и что необходимо сделать для минимизации этих рисков и проблем. Если бы у вас был выбор, то создавали бы вы гетерогенную среду или нет? Мотивируйте ваш ответ примерами.
- Основная проблема гетерогенной системы виртуализации - несовместимость ВМ между гипервизорами в чистом виде. Для оперативных переносов ВМ между серверами необходимо конвертировать образы виртуальных дисков в формат целевого гипервизора. Поэтому, для более гибкого управления инфраструктурой лучше применять гомогенную структуру используемых платформ. На нашем предприятии, например, используется продукт VMWare ESXi серверы которого подключены оптикой к 2м СХД. Управление различными ВМ в такой конфигурации очеь простое и прозрачное.
