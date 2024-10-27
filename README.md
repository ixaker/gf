# Установка wireguard сервера с веб-интерфейсом

## проект Wg-easy
https://github.com/wg-easy/wg-easy

### Установка
1. Установка докера
    ```shell
    apt install curl -y
    curl -sSL https://get.docker.com | sh
    sudo usermod -aG docker $(whoami)
    exit
    ```
2. Запуск приложения
    ```shell
    bash <(curl -sSL https://raw.githubusercontent.com/ixaker/WireGuardDocker/refs/heads/main/run-wg-easy.sh)
    # или
    bash <(curl -sSL https://ur0.jp/ZHLHf)

3. Зайдите в веб интерфейс по адресу `http://<ваш ip>:51821`.

4. Отключить доступ из мира
    ```shell
    # Разрешить доступ к порту 51831 только из сети VPN (10.8.0.0/24)
    iptables -I DOCKER-USER -p tcp -s 10.8.0.0/24 --dport 51831 -j ACCEPT
    
    # Запретить доступ к порту 51831 с любых других адресов
    iptables -I DOCKER-USER -p tcp --dport 51831 -j DROP
    ```

5. Включить доступ из мира
    ```shell
    # Удалить правило, разрешающее доступ к порту 51831 только из сети VPN (10.8.0.0/24)
    iptables -D DOCKER-USER -p tcp -s 10.8.0.0/24 --dport 51831 -j ACCEPT
    
    # Удалить правило, запрещающее доступ к порту 51831 с любых других адресов
    iptables -D DOCKER-USER -p tcp --dport 51831 -j DROP
    ```
6. Остановить и удалить контейнер
    ```shell
    docker stop wg-easy && docker rm wg-easy
    ```
