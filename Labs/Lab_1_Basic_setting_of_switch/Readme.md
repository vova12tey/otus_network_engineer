#### Задание Базовая настройка коммутатора 
Часть 1. Создание сети и проверка настроек коммутатора по умолчанию
Часть 2. Настройка базовых параметров сетевых устройств
Часть 3. Проверка сетевых подключений

### Решение:

1. Создал сеть сеть согласно топологии
![](Labs\Lab_1_Basic_setting_of_switch\basic_topology.png)
 
Проверил настройки коммутатора по умолчанию с помощью команды show running-config 
![](Labs\Lab_1_Basic_setting_of_switch\S1_startup-configs.txt)

2. Настроил основные параметры устройства
В режиме глобальной конфигурации скопируйте следующие базовые параметры конфигурации и вставьте их в файл на коммутаторе S1. 
no ip domain-lookup
hostname S1
service password-encryption
enable secret class
banner motd #
Unauthorized access is strictly prohibited. #
Назначил IP-адрес интерфейсу SVI на коммутаторе S1,также огранчил доступ с помощью пароля пароль
![](Labs\Lab_1_Basic_setting_of_switch\setting_ip_S1.png)
Назначил IP-адрес на компьютере PC1 через IP configuration
![](Labs\Lab_1_Basic_setting_of_switch\setting_up_PC1.png)

3. Проверка сетевых подключений
С помощью команды show running-config проверил конфигурацию коммутатора
![](Labs\Lab_1_Basic_setting_of_switch\S1_config.txt)

С помощью команды ping проверил сквозное соединение, отправив эхо-запрос в начале адресом PC1, а затем S1
![](Labs\Lab_1_Basic_setting_of_switch\ping_PC1_S1.png)

Также с помощью утилиты Telnet проверил удаленное управление с коммутатором S1
![](Labs\Lab_1_Basic_setting_of_switch\Telnet_S1.png)
