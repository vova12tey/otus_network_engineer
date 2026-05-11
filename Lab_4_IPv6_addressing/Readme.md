Задачи:

Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора
Назначил имя хоста и настройте основные параметры маршрутизатора
<img width="949" height="1031" alt="Screenshot_224" src="https://github.com/user-attachments/assets/e273ca59-c697-450a-9255-29cc8252d445" />
<img width="886" height="1026" alt="Screenshot_222" src="https://github.com/user-attachments/assets/e9ccf3ba-130f-4320-a0a2-cdaeabe12bef" />
Назначил имя хоста и настройте основные параметры коммутатора.

<img width="757" height="1032" alt="Screenshot_228" src="https://github.com/user-attachments/assets/cc2aebdb-dd6f-4f13-b5bb-3637d447df9f" />
Часть 2. Ручная настройка IPv6-адресов
Назначил глобальные индивидуальные IPv6-адреса, указанные в таблице адресации обоим интерфейсам Ethernet на R1
<img width="889" height="1030" alt="Screenshot_227" src="https://github.com/user-attachments/assets/4edc7cc3-03d6-4084-a1a8-550d6cb156ff" />

Назначил адрес IPv6 для S1, а также назначил этому интерфейсу локальный адрес канала fe80::b
<img width="757" height="1032" alt="Screenshot_228" src="https://github.com/user-attachments/assets/6de46ec5-6890-4833-9ed6-9a75a8a124d7" />

Часть 3. Проверка сквозного соединения
Отправил эхо-запрос с PC-A на FE80::1. Это локальный адрес канала, назначенный G0/1 на R1.
<img width="753" height="1022" alt="Screenshot_229" src="https://github.com/user-attachments/assets/7d1cd524-e3e3-4f4e-9318-3ceb45111408" />

Отправил эхо-запрос на интерфейс управления S1 с PC-A. При вводе команды tracert на PC-A получился такой результат.
<img width="759" height="1013" alt="Screenshot_233" src="https://github.com/user-attachments/assets/7efeacf4-4a36-4ee5-a048-6e021824f577" />
Значит ли данный результат отсутствием сквозного подключения?
Проверил и IPv6-адреса указаны на всех устройствах верно.

Отправил эхо-запрос с PC-B  на PC-A.
<img width="757" height="1015" alt="Screenshot_234" src="https://github.com/user-attachments/assets/fb273812-8588-41fb-ad69-da94f9750620" />

Отправил эхо-запрос с PC-B  на локальный адрес канала G0/0 на R1.
<img width="747" height="1010" alt="Screenshot_236" src="https://github.com/user-attachments/assets/d9c066c3-dccc-4ba9-a4ec-814e2bd13280" />
<img width="747" height="1006" alt="Screenshot_235" src="https://github.com/user-attachments/assets/1a3d79d0-518e-41da-96b4-344f318ab63f" />



