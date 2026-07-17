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

Вопрос из лабораторной работы: Откуда взялась часть адреса с идентификатором хоста?

Ответ: Идентификатор хоста (часть адреса после acad:1:) генерируется одним из двух способов:

EUI-64 — на основе MAC-адреса сетевой карты (если отключены Privacy Extensions).
Случайным образом — по умолчанию в Windows используются временные (Temporary) адреса для повышения приватности (Privacy Extensions), что и видно в выводе ipconfig.

<img width="878" height="885" alt="Screenshot_14" src="https://github.com/user-attachments/assets/35e5c9a9-aa94-477f-8f5f-272267cc7948" />

<img width="875" height="886" alt="Screenshot_15" src="https://github.com/user-attachments/assets/da26be50-3e62-4aab-82ff-ee9ce7f32896" />


Часть 3. Настройка и проверка сервера DHCPv6 без гражданства на R1

Часть 4. Настройка и проверка состояния DHCPv6 сервера на R1

Часть 5. Настройка и проверка DHCPv6 Relay на R2
