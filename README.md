Данная инструкция по установке является частным случаем применения проекта  Waujito youtubeUnblock https://github.com/Waujito/youtubeUnblock.
Проверено на нескольких роутерах ASUS RT-AX56U.

Если ранее устанавливали даннй пакет, инструкция по обновлению есть ниже пункт 2.

1. Краткая инструкциия:

Необходим роутер с прошивкой [Asuswrt-Merlin](https://www.asuswrt-merlin.net/), на котором установлен [Entware](https://github.com/RMerl/asuswrt-merlin.ng/wiki/Entware).
Соединяемся с роутером по SSH, я использую PuTTY. Вводим логин и пароль на вход роутера.
Далее в коммандной строке исполняем комманды, если спрашивает подтверждение, подтверждаем нажатием "Y".

Доставляем необходимые пакеты для тех кто ставит первый раз. Для тех кто обновляется после пункта 2. Эти пакеты уже установлены. Повторно ставить не надо.
```
opkg install procps-ng-pgrep
opkg install daemonize
```
Закачиваем пакет и устанавливаем
```
wget https://github.com/Di1Ly/OdroidGOAtari800/releases/download/New2/youtubeUnblock_1.0.0-e9b033c-armv7-3.2-1_armv7-3.2.ipk
opkg install youtubeUnblock_1.0.0-e9b033c-armv7-3.2-1_armv7-3.2.ipk
```

Запускаем наш скрипт в режиме демона
```
daemonize /opt/etc/init.d/S91youtubeUnblock start
```
В принципе, в таком виде должно работать.
Для проверки можно узнать статус
```
/opt/etc/init.d/S91youtubeUnblock status
```
Ответ должен быть:
```
running
```
Будет работать до перезагрузки роутера
Думаю необходимо добавить запуск так: (НО ЭТО НАДО ПРОВЕРЯТЬ!!!)
```
echo 'daemonize . /opt/etc/init.d/S91youtubeUnblock start' >> /jffs/scripts/post-mount
```
скрипт пропишется строчкой запуска при старте роутера.
Вроде все.

2. Инструкция по обновению с прошлой версии
Останавливаем скрипт
```
/opt/etc/init.d/S91youtubeUnblock stop
```
```
wget https://github.com/Di1Ly/OdroidGOAtari800/releases/download/New2/youtubeUnblock_1.0.0-e9b033c-armv7-3.2-1_armv7-3.2.ipk
```
opkg list-installed
```
Находим в списке пакет у которого начало содержит что то типа:
```
youtubeUnblock - 0.3.2-3d50c00-1
```
Удаляем старый пакет
```
opkg remove youtubeUnblock - 0.3.2-3d50c00-1
```
Переходи к пункту 1. (в начале инструкции).

