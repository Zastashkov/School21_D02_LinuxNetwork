# DO2_LinuxNetwork-0

## Part 1. Инструмент ipcalc

### 1.1. Сети и маски

**1) Адрес сети ip *192.167.38.54/13***

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled.png)

- Адрес сети: 192.160.0.0

**2) Перевод маски *255.255.255.0* в префиксную и двоичную запись, */15* в обычную и двоичную, *11111111.11111111.11111111.11110000* в обычную и префиксную**

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%201.png)

- Перевод маски 255.255.255.0
    - *Префиксная запись* - 24
    - *Двоичная запись* - 11111111.11111111.11111111. 00000000

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%202.png)

- Перевод маски /15
    - *Обычная запись* ****- 255.254.0.0
    - *Двоичная запись* - 11111111.1111111 0.00000000.00000000

**3) Минимальный и максимальный хост в сети *12.167.38.4* при масках:** 

***/8*, *11111111.11111111.00000000.00000000*, *255.255.254.0* и */4***

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%203.png)

- *При маске /8*
    - *Min: 12.0.0.1*
    - *Max: 12.255.255.254*
- *При маске 11111111.11111111.00000000.00000000*
    - *Min: 12.167.0.1*
    - *Max: 12.167.255.254*
- *При маске 255.255.254.0*
    - *Min: 12.167.38.1*
    - *Max: 12.167.39.254*
- *При маске /4*
    - *Min: 0.0.0.1*
    - *Max: 15.255.255.254*

### 1.2. localhost

Для доступа к приложению, работающему на localhost ip адрес устройства должен входить в промежуток 127.0.0.1 — 127.255.255.254. 

- 194.34.23.100 - не localhost
- 127.0.0.2 - localhost
- 127.1.0.1 - localhost
- 128.0.0.1 - не localhost

### 1.3. Диапазоны и сегменты сетей

1) К частным адресам относятся IP-адреса из следующих подсетей:

- От 10.0.0.0 до 10.255.255.255 с маской 255.0.0.0 или /8
- От 172.16.0.0 до 172.31.255.255 с маской 255.240.0.0 или /12
- От 192.168.0.0 до 192.168.255.255 с маской 255.255.0.0 или /16
- От 100.64.0.0 до 100.127.255.255 с маской подсети 255.192.0.0 или /10
    - *10.0.0.45 -* частный
    - *134.43.0.2 -* публичный
    - *192.168.4.2 -* частный
    - *172.20.250.4 -* частный
    - *172.0.2.1 -* публичный
    - *192.172.0.1 -* публичный
    - *172.68.0.2 -* публичный
    - *172.16.255.255 -* частный
    - *10.10.10.10 -* частный
    - *192.169.168.1 -* публичный

2) Все адреса входящие в промежуток между HostMin и HostMax подходят:

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%204.png)

- *10.0.0.1 - Не подходит*
- *10.10.0.2* - Подходит
- *10.10.10.10* - Подходит
- *10.10.100.1* - Не подходит
- *10.10.1.255* - Подходит

## **Part 2. Статическая маршрутизация между двумя машинами**

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%205.png)

- Вывод `ip -a` на ws1

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%206.png)

- Вывод `ip -a` на ws2
- Описание внутреннего сетевого интерфейса приведено на скрине под цифрой 1

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%207.png)

- На ws1 и ws2 отключаем DHCP, меняем адрес и маску на соответсвующие в задании. Прописываем настройки адаптера enp0s8, добавляем его в настройках виртуальной машины с названием intnet (на обоих машинах).

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%208.png)

- Применяем изменения, используя команду `sudo netplan apply`.

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%209.png)

- Появился новый адаптер с ip 192.168.100.10 и маской /16 на первой машине, на второй  ip 172.24.116.8/12

### 2.1. **Добавление статического маршрута вручную**

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2010.png)

- Команда `sudo ip route add 172.24.116.8 dev enp0s8` для создания маршрута на ws1

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2011.png)

- Команда `sudo ip route add 192.168.100.10 dev enp0s8` для создания маршрута на ws2

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2012.png)

- Пинги прошли успешно

### 2.2. Добавление статического маршрута с сохранением

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2013.png)

- Добавляем статические маршруты

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2014.png)

- Пинг прошел успешно

## Part 3. Утилита iperf3

### 3.1. Скорость соединения

- 8 Mbps - 1 MB/s (Мегабит/сек в Мегабайт/сек)
- 100 MB/s - 819200 Kbps, (Мегабайт/сек в Килобит/сек)
- 1 Gbps - 1024 Mbps (Гигабит/сек в Мегабит/сек)
- [Крутой конвертер единиц](https://convertlive.com/ru/%D0%BA%D0%BE%D0%BD%D0%B2%D0%B5%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D1%82%D1%8C)

### 3.2. Утилита iperf3. Измерить скорость между ws1 и ws2

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2015.png)

- Сначала запускаем команду `iperf3 -s` на виртуалке, которая выступает в роли сервера, затем команду `iperf -c 192.168.100.10` (ip сервера) на клиенте.

## **Part 4. Сетевой экран**

### **4.1. Утилита iptables**

 Документация по [iptables](https://www.opennet.ru/docs/RUS/iptables/)

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2016.png)

- Содержимое файла `/etc/firewall.sh` на обеих машинах. Разница заключается в порядке команд, утилита iptables выполняет первое прочитанное правило, соответсвенно на ws1 будет выполнятся запрет и пинг не пройдет, а на ws2 наоборот, первым стоит ACCEPT, разрешить прохождение пакета, пинг пройдет.

### Запуск скриптов

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2017.png)

- `sudo chmod +x /etc/firewall.sh` - выдача прав и запуск  `sudo sh /etc/firewall.sh`

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2018.png)

- `sudo iptables -L` - Просмотр всех правил во всех цепочках

### **4.2. Утилита nmap**

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2019.png)

- Вывод команды nmap

## **Part 5. Статическая маршрутизация сети**

### **5.1. Настройка адресов машин**

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2020.png)

- Содержимое файла *etc/netplan/00-installer-config.yaml*
 для ws11

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2021.png)

- Содержимое файла *etc/netplan/00-installer-config.yaml*
 для ws21

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2022.png)

- Содержимое файла *etc/netplan/00-installer-config.yaml*
 для ws22

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2023.png)

- Содержимое файла *etc/netplan/00-installer-config.yaml*
 для r1

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2024.png)

- Содержимое файла *etc/netplan/00-installer-config.yaml*
 для r2

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2025.png)

- Пинг ws22 с ws21

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2026.png)

- Пинг r1 с ws11
- **Дальше `ip -4 a` для каждой машины**

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2027.png)

### **5.2. Включение переадресации IP-адресов.**

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2028.png)

- Для включения переадресации IP на роутерах (до следующей перезагрузки) используется команда `sysctl -w net.ipv4.ip_forward=1`

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2029.png)

- Измененные файлы `/etc/sysctl.conf` на r1 и r2 для включения IP-переадресации на постоянной основе

### **5.3. Установка маршрута по-умолчанию**

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2030.png)

- На ws21 и ws22 аналогично добавляем `gateway4` перед routes, только с IP второго шлюза (10.10.0.2)

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2031.png)

- Маршруты до шлюзов добавились 👍

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2032.png)

- Ping с ws11 до r2

### **5.4. Добавление статических маршрутов**

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2033.png)

- Статические маршруты в r1 и r2

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2034.png)

- Добавленные статические маршруты на r1 и r2

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2035.png)

- Запуск команд `ip r list 10.10.0.0/18` и `ip r list 0.0.0.0/0` для`ws11`.
- Маршрут был выбран отличный, поскольку процесс оценки маршрута в каждом маршрутизаторе использует метод совпадения самого длинного префикса для получения наиболее точного маршрута. Сеть с самой длинной маской подсети или префиксом сети, которая соответсвует целевому ip-адресу, является сетевым шлюзом следующего перехода. Процесс повторяется до тех пор, пока пакет не будет доставлен на хост назначения. 
**Если вкратце, при наличии двух и более маршрутов выбирается маршрут с самой длинной маской т.к. он более точный**

### **5.5. Построение списка маршрутизаторов**

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2036.png)

- *Запуск команды `traceroute` на `ws11`.*
- Каждый пакет проходит на своем пути определенное количество узлов, пока достигнет своей цели. Причем, каждый пакет имеет свое время жизни. Это количество узлов, которые может пройти пакет перед тем, как он будет уничтожен. Этот параметр записывается в заголовке TTL, каждый маршрутизатор, через который будет проходить пакет уменьшает его на единицу. При TTL=0 пакет уничтожается, а отправителю отсылается сообщение Time Exceeded.Команда traceroute linux использует UDP пакеты. Она отправляет пакет с TTL=1 и смотрит адрес ответившего узла, дальше TTL=2, TTL=3 и так пока не достигнет цели. Каждый раз отправляется по три пакета и для каждого из них измеряется время прохождения. Пакет отправляется на случайный порт, который, скорее всего, не занят. Когда утилита traceroute получает сообщение от целевого узла о том, что порт недоступен трассировка считается завершенной.

### 5.6. Использование протокола ICMP при маршрутизации

**Запустить на r1 перехват сетевого трафика, проходящего через eth0 с помощью команды:**

`tcpdump -n -i eth0 icmp`

**Пропинговать с ws11 несуществующий IP (например, *10.30.0.111*) с помощью команды:**

`ping -c 1 10.30.0.111`

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2037.png)

## ****Part 6. Динамическая настройка IP с помощью DHCP****

Сначала `sudo apt install isc-dhcp-server`, после этого переходим в файл `sudo vim /etc/dhcp/dhcpd.conf` и прописываем там настройки:

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2038.png)

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2039.png)

- Настройки файла *resolv.conf*
- Перезагружаем dhcp командой `systemctl restart isc-dhcp-server`

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2040.png)

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2041.png)

- Ставим true напротив dhcp, статические настройки комментим

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2042.png)

- Присвоенный ws22 ip адрес входит в тот диапазон dhcp, который мы указали в `/etc/dhcp/dhcpd.conf`

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2043.png)

- Пинг ws22 с ws21

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2044.png)

- Указываем mac адрес у ws11

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2045.png)

- После того как поменяли mac-адрес в конфигах, нужно изменить его и в настройках виртуальной машины, иначе она не загрузится
- Теперь настраиваем r2 по тому же принципу, за исключением но теперь с “жесткой“ приязкой к mac-адресу хоста ws11

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2046.png)

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2047.png)

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2048.png)

- Тесты на видимость в локальной сети работают, но отсутствует доступ в интернет. Если переписать приоритет дефолтных шлюзов, можно сменить адаптер на NAT, но тогда локальная сеть перестанет работать. В целом, DNS сервер в файле resolv.conf не имеет смысла, поскольку файл переписывается каждый раз когда происходит рестарт dhcp.

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2049.png)

- Запрос на выдачу ip-адреса у dhcp сервера
- *dhclient -r* - удаление старых (всех) ip;

```sql
subnet 10.20.0.0 netmask 255.255.255.192
{
    range 10.20.0.2 10.20.0.50; - диапазон доступных IP адресов
    option routers 10.20.0.1; - адрес шлюза маршрутизатора
    option domain-name-servers 10.20.0.1; IP адресс DNS-сервера
}
```

- [Видос по настройке dhcp](https://www.youtube.com/watch?v=HDpCo7DvsgY)

## Part 7. NAT

- Сначала ставим apach `sudo apt install apache2`

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2050.png)

- Изменяем строку Listen 80 на Listen 0.0.0.0:80 (на ws22), тем самым делаем сервер общедоступным

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2051.png)

- То же самое но на r1

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2052.png)

- Запускаем веб-сервер Apache командой `service apache2 start`

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2053.png)

- 1, 2, 3) Добавляем правила в firewall на r2 в соответсвии с заданием

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2054.png)

- Запуск скрипта

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2055.png)

- Пинг с ws22 до r1 не проходит

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2056.png)

- 4) Добавляем еще одно правило для пропуска icmp

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2057.png)

- После добавления правила, разрешающего icmp, пинг с ws22 проходит до r1

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2058.png)

- 5, 6) Включаем snat, dnat, добавляем правило, которое разрешает tcp

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2059.png)

- Коннектимся к r1 через telnet на ws22 (проверка SNAT)

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2060.png)

- Коннектимся к ws22 через telnet на r1 (проверка DNAT)

## ****Part 8. Дополнительно. Знакомство с SSH Tunnels****

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2061.png)

- Меняем в `/etc/apache2/ports.conf` порт на local

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2062.png)

- Запускаем apache на ws22 с новыми настройками

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2063.png)

- Проверка работы сервера

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2064.png)

- Команды необходимые для выполнения этих четырех пунктов:
    
    1) `service apache2 start` - запуск сервера на ws22
    
    2) `ssh -L 8080:10.20.0.20:80 daniil2@localhost` - доступ с ws21 к серверу на ws22
    
    3) `ssh -R 8080:10.20.0.20:80 daniil2@localhost` - доступ с ws21 к серверу на ws22
    
         (либо `ssh -L 9999:localhost:80 daniil2@10.20.0.20` и 
    
                    `ssh -R 9999:localhost:80 daniil2@10.20.0.20` соответсвенно)
    
    4) `telnet 127.0.0.1 8080` - проверка подключения
    
- Подключаемся к ws22 с ws21

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2065.png)

- Подключаемся к ws22 с ws11

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2066.png)

- Проверка подключения

![Untitled](DO2_LinuxNetwork-0%204f06c2733feb471caf0edf23da688ab5/Untitled%2067.png)