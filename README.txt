Бутстрап очень простой git clone конфигов на свой хост.
Для запуска вагранта нужно установить провайдер, самый простейший virtualbox.
И в директории с Vagrantfile выполнить vagrant up - у вагрант файл уже встроенный 
провиженинг через ансибл на первый хост.
После испольние команды получим готовый сентри сервер и 2 сервер для бекапа.


Security hardering and tuning OS
1. Cоздаем пользователя под которым будем работать
# adduser user
Пишем сложный пароль
2. Разрешаем user выполнение sudo
Добавляем user в специальную группу sudo:
# usermod -a -G sudo user
3. Сконфигурируем ssh в файле /etc/ssh/sshd_config
Разрешим соединение для определенных типов адрессов
AddressFamily inet
Запретим авторизацию под root:
PermitRootLogin no
Разрешаем подключение только по определенным логинам:
AllowUsers user
Запрещаем попытку входа с пустым паролем:
PermitEmptyPasswords no
3. Конфигурируем /etc/sysctl.conf
# Защита от smurf-атак
net.ipv4.icmp_echo_ignore_broadcasts = 1
# Защита от неправильных ICMP-сообщений
net.ipv4.icmp_ignore_bogus_error_responses = 1
# Защита от SYN-флуда
net.ipv4.tcp_syncookies = 1
# Запрещаем маршрутизацию от источника
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0
# Защита от спуфинга
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1
# Блокируем форвардинг трафика так как наш хост не маршрутизатор
net.ipv4.ip_forward = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
#ускоряем высвобождение памяти
vm.swappiness=10
4. Настраиваем фаервол iptables -  
INPUT chain[default ACCEPT]
разрешаем 80 порт для всех и 22 порт для  ssh трафика 
для своей локальной сети или для определенных хостов
Other DROP
OUTPUT ACCEPT all
FORWARDING - DROP all
5. Настраиваем /etc/security/limit.conf
Выставить максимальное количество на открытые файлы и максимальное количество процессов которые 
могут быть созданы пользователем
ulimit -u unlimited - настроить для рута неограниченное число процессов




