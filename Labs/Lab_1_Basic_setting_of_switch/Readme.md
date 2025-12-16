### Задание:
Часть 1. Создание сети и проверка настроек коммутатора по умолчанию

Часть 2. Настройка базовых параметров сетевых устройств

Часть 3. Проверка сетевых подключений

### Решение:

1. Создал сеть сеть согласно топологии
<img width="951" height="1033" alt="basic_topology" src="https://github.com/user-attachments/assets/50f66ca9-5f20-4a2c-ab98-cc5de2731960" />

Проверил настройки коммутатора по умолчанию с помощью команды show running-config 
Labs\Lab_1_Basic_setting_of_switch\S1_startup-configs

2. Настроил основные параметры устройства
В режиме глобальной конфигурации скопируйте следующие базовые параметры конфигурации и вставьте их в файл на коммутаторе S1. 
no ip domain-lookup
hostname S1
service password-encryption
enable secret class
banner motd #
Unauthorized access is strictly prohibited. #
Назначил IP-адрес интерфейсу SVI на коммутаторе S1,также ограничил доступ с помощью пароля пароль
<img width="697" height="1021" alt="setting_ip_S1" src="https://github.com/user-attachments/assets/d59d35dc-4956-482e-8456-a852820ef504" />

![](Labs\Lab_1_Basic_setting_of_switch\setting_ip_S1.png)
Назначил IP-адрес на компьютере PC1 через IP configuration
<img width="1324" height="895" alt="setting_up_PC1" src="https://github.com/user-attachments/assets/3f3a2c6d-5b6c-4506-9d21-7e085f4c3198" />


![](Labs\Lab_1_Basic_setting_of_switch\setting_up_PC1.png)

3. Проверка сетевых подключений
С помощью команды show running-config проверил конфигурацию коммутатора
Labs\Lab_1_Basic_setting_of_switch\S1_config

С помощью команды ping проверил сквозное соединение, отправив эхо-запрос в начале адресом PC1, а затем S1
<img width="696" height="1017" alt="ping_PC1_S1" src="https://github.com/user-attachments/assets/0ae8229d-e75b-4cb8-b174-7f3055b75f42" />
![](Labs\Lab_1_Basic_setting_of_switch\ping_PC1_S1.png)

Также с помощью утилиты Telnet проверил удаленное управление с коммутатором S1
<img width="864" height="894" alt="Telnet_S1" src="https://github.com/user-attachments/assets/e0cb2636-dc0c-4455-b26c-acfc561d24f1" />

![](Labs\Lab_1_Basic_setting_of_switch\Telnet_S1.png)
