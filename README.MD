# Запуск

1. Зайти на сервер по ssh от имени **root**
2. Ввести команду
```bash
curl -sL "https://raw.githubusercontent.com/shinikakeru/Auto-Set-VPS/main/vps.sh?cache=$(date +%s)" > setup.sh && chmod +x setup.sh && sudo ./setup.sh
```
3. Все выполнится автоматически **(Добавить ПУБЛИЧНЫЙ ключ нужно вручную)**

## Изображения работы
<p align="center">
  <img src="example1.jpg">
</p>

<p align="center">
  <img src="example2.jpg">
</p>

<p align="center">
  <img src="example3.jpg">
</p>


## Что делает скрипт

<table>
  <tr><td align="center">1.</td><td>Настройка часового пояса на <b>Москву</b></td></tr>
  <tr><td align="center">2.</td><td>Включение <b>TCP BBR</b></td></tr>
  <tr><td align="center">3.</td><td>Переадресация IP</td></tr>
  <tr><td align="center">4.</td><td>Защита от сетевых атак (SYN-flood, IP spoofing)</td></tr>
  <tr><td align="center">5.</td><td>Защита от ICMP-манипуляций</td></tr>
  <tr><td align="center">6.</td><td>Увеличение буферов сети для высокой скорости <b>VPN</b></td></tr>
  <tr><td align="center">7.</td><td>Меняет порт на 1024 для ssh в <b>/etc/ssh/sshd_config</b></td></tr>
  <tr><td align="center">8.</td><td>Добавляет SSH ключ в <b>/root/.ssh/authorized_keys</b> <b>(опционально)</b></td></tr>
  <tr><td align="center">9.</td><td>Отключает/Включает вход по паролю <b>(опционально только для подключения по SSH ключу)</b></td></tr>
  <tr><td align="center">10.</td><td>Скрывает баннеры <b>PrintMotd</b> и <b>PrintLastLog</b>, которые идут по умолчанию в <b>/etc/ssh/sshd_config</b></td></tr>
  <tr><td align="center">11.</td><td>Создает свой баннер</td></tr>
  <tr><td align="center">12.</td><td>Автоматически настраивает фаервол, оставляет открытым <b>только порт для ssh</b></td></tr>
  <tr><td align="center">13.</td><td>Скачивает и настраивает fail2ban для <b>1024 порта</b></td></tr>
  <tr><td align="center">14.</td><td>Выводит текущие ключи на сервере в <b>/root/.ssh/authorized_keys</b></td></tr>
  <tr><td align="center">15.</td><td>Перезапускает службы</td></tr>
  <tr><td align="center">16.</td><td>Чистит файлы за собой</td></tr>
</table>

## Полезные команды
**Для powershell**
**1. Подключиться по SSH с указанием ключа и порта 1024 (если имя ключа по умолчанию)**
```powershell
ssh root@IP_СЕРВЕРА -p 1024
```
**Если ключ назван по-своему**
```powershell
ssh -i $HOME\.ssh\имя_вашего_ключа root@IP_СЕРВЕРА -p 1024
```
**2. Создаст ключ формата ed25519 с вашим названием по пути `C:\Users\Имя_Пользователя\.ssh\`**
```powershell
ssh-keygen -t ed25519 -f $HOME\.ssh\имя_вашего_ключа
```
**3. Сразу открыть публичный ключ**
```powershell
cat $HOME\.ssh\имя_вашего_ключа.pub
```
**4. Если вы создаете ключ впервые, папки .ssh может еще не существовать. В таком случае команда ssh-keygen может выдать ошибку. Если это произойдет, сначала создайте папку командой**
```powershell
mkdir $HOME\.ssh
```

**Для Linux (на сервере)**
**1. Проверить работы BBR (должно вернуть bbr)**
```bash
sysctl net.ipv4.tcp_congestion_control
```
**2. Проверить IP Forwarding для VPN (должно вернуть 1)**
```bash
sysctl net.ipv4.ip_forward
```
**3. Проверка работы ufw**
```bash
sudo ufw status
```
**4. Проверка работы fail2ban**
```bash
sudo fail2ban-client status sshd
```
**5. Разблокировать забаненный IP в fail2ban**
```bash
sudo fail2ban-client set sshd unbanip ВАШ_IP
```
**6. Смотреть логи подключений по SSH в реальном времени**
```bash
journalctl -u ssh -f
```
