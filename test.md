Ghbdtn
# Ghbdtn

Итак, заходим на сервер и скачиваем репозиторий XTLS/Xray-install:

bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
Оттуда же обновляем geoip.dat и geosite.dat:

bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install-geodata

Скопируем конфиг в рабочую область:

sudo cp config.json /usr/local/etc/xray/config.json
Наконец, запускаем сервис

sudo systemctl start xray
Смотрим, чтобы не было ошибок. При наличии - ищем их в файле конфигурации.

sudo systemctl status xray
Ну и если всё заработало, создадим сервис:

sudo systemctl enable xray
Запуск и использование прокси
Порты узнаем из конфига в облости inbounds проверь порты 
Комманда для поиска jq -r '.inbounds[] | "\(.tag): \(.port)"' /usr/local/etc/xray/config.json
Для HTTP соединений используем порт 2081, для SOCKS 2080. В конфиге, естественно, можно поменять порты на любые.

export http_proxy=http://127.0.0.1:2081
export https_proxy=http://127.0.0.1:2081
export ftp_proxy=http://127.0.0.1:2081
export socks_proxy=http://127.0.0.1:2080
Если включён ufw на сервере, откроем порты:

sudo ufw allow 2080/tcp
sudo ufw allow 2081/tcp
sudo iptables -A INPUT -p tcp --dport 2080 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 2081 -j ACCEPT
Использование SOCKS прокси:

export SOCKS_PROXY="socks5://<IP-adress>:2080"
HTTP прокси:

export http_proxy="http://<IP-adress>:2081"
