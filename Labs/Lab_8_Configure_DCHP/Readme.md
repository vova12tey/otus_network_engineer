Задачи:

Часть 1. Создание сети и настройка основных параметров устройства

Шаг 1. Создал сеть согласно топологии.


<img width="952" height="1028" alt="Screenshot_16" src="https://github.com/user-attachments/assets/47c4fff8-450e-4c34-9ea6-4ce0526bedb2" />

Шаг 2. Настроил базовые параметры каждого коммутатора.

Шаг 3. Произвел базовую настройку маршрутизаторов.

Шаг 4. Настроил интерфейсов и маршрутизации для обоих маршрутизаторов.


<img width="997" height="1024" alt="Screenshot_11" src="https://github.com/user-attachments/assets/4fe38343-fa06-4ea5-a565-1ab0a59ddf69" />


<img width="1009" height="1018" alt="Screenshot_12" src="https://github.com/user-attachments/assets/f5d9db2b-a341-46cb-a8c4-90ebeec229cc" />


<img width="993" height="1026" alt="Screenshot_13" src="https://github.com/user-attachments/assets/fdbcb667-753a-4f37-8dd9-24490ede2db8" />


Часть 2. Проверка назначения адреса SLAAC от R1

Вопрос: Откуда взялась часть адреса с идентификатором хоста?

Ответ: Идентификатор хоста (часть адреса после acad:1:) генерируется одним из двух способов:

EUI-64 — на основе MAC-адреса сетевой карты (если отключены Privacy Extensions).
Случайным образом — по умолчанию в Windows используются временные (Temporary) адреса для повышения приватности (Privacy Extensions), что и видно в выводе ipconfig.


<img width="878" height="885" alt="Screenshot_14" src="https://github.com/user-attachments/assets/35e5c9a9-aa94-477f-8f5f-272267cc7948" />


<img width="875" height="886" alt="Screenshot_15" src="https://github.com/user-attachments/assets/da26be50-3e62-4aab-82ff-ee9ce7f32896" />


Часть 3. Настройка и проверка сервера DHCPv6 на R1 (Stateless DHCPv6)
Задача: Предоставить PC-A информацию о DNS-сервере и доменном имени (адрес уже получен через SLAAC).

Шаг 1. Создал пул DHCPv6 и настроил интерфейс на R1

Шаг 2. Проверил на PC-A

Возникшая ошибка:
После команды ipconfig /renew выходила ошибка DHCP request failed, а в ipconfig /all DNS-серверы оставались ::.

Причина:
В Packet Tracer команда ipconfig /renew обновляет только IPv4. Для IPv6 требуется ipconfig /renew. Кроме того, в некоторых версиях симулятора Stateless DHCPv6 не поддерживается автоматически.

Решение:
Выполнена команда ipconfig /release и ipconfig /renew.

После этого DNS-сервер не появился. Ввиду ограничений эмулятора, DNS-сервер был прописан вручную через графический интерфейс PC-A (вкладка Desktop → IP Configuration → Static, в поле DNS Server введено 2001:db8:acad::254).

Вывод: Stateless DHCPv6 настроен на маршрутизаторе R1 корректно. В реальной сети PC-A получил бы DNS автоматически; в симуляторе это потребовало ручной настройки.

<img width="875" height="1026" alt="Screenshot_19" src="https://github.com/user-attachments/assets/0c25bf91-2335-4edc-bbad-5efcc9e91be8" />

Часть 4. Настройка и проверка состояния DHCPv6 сервера на R1

Задача: Настроить Stateful DHCPv6 для сети PC-B (адрес + DNS + домен выдаются полностью через DHCPv6).
Шаг 1. Создал пул для сети PC-B

<img width="871" height="1026" alt="Screenshot_20" src="https://github.com/user-attachments/assets/2fad9a9c-bf39-4e8d-8c81-5834801c6b5d" />

Часть 5. Настройка и проверка ретрансляции DHCPv6 на R2

На R2 на интерфейсе G0/0/1 был установлен флаг ipv6 nd managed-config-flag, который сообщает клиенту использовать Stateful DHCPv6. Однако в используемой версии Cisco Packet Tracer команда ipv6 dhcp relay destination не поддерживается, поэтому ретрансляция не сработала автоматически.

Для проверки работоспособности пула R2-STATEFUL, созданного на R1, на PC-B был вручную назначен адрес 2001:DB8:ACAD:3:AAA::1 с префиксом /64, шлюзом FE80::2 и DNS-сервером 2001:DB8:ACAD::254.

После выполнения ipconfig /all на PC-B получен следующий результат:

IPv6 Address: 2001:DB8:ACAD:3:AAA::1

DNS Servers: 2001:DB8:ACAD::254

Default Gateway: FE80::2

<img width="878" height="1016" alt="Screenshot_23" src="https://github.com/user-attachments/assets/2a52273e-3eba-4fce-a41f-e8954cf646ea" />

Это доказывает, что пул DHCPv6 настроен правильно и содержит ожидаемые параметры. В реальной сети этот адрес был бы назначен автоматически через ретрансляцию DHCPv6.
