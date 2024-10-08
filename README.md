Данная инструкция по установке является частным случаем применения проекта  Waujito youtubeUnblock https://github.com/Waujito/youtubeUnblock.
Проверено на нескольких роутерах ASUS RT-AX56U.

Краткая инструкциия:

Необходим роутер с прошивкой [Asuswrt-Merlin](https://www.asuswrt-merlin.net/), на котором установлен [Entware](https://github.com/RMerl/asuswrt-merlin.ng/wiki/Entware).
Соединяемся с роутером по SSH, я использую PuTTY. Вводим логин и пароль на вход роутера.
Далее в коммандной строке исполняем комманды, если спрашивает подтверждение, подтверждаем нажатием "Y".

```
opkg install procps-ng-pgrep
opkg install daemonize
wget https://github.com/Di1Ly/OdroidGOAtari800/releases/download/New/youtubeUnblock_0.3.2-3d50c00-1_armv7-3.2.ipk
opkg install youtubeUnblock_0.3.2-3d50c00-1_armv7-3.2.ipk
```
после установки пакета, необходимо поправить скрипт, а отчнее функцию в нем
```
nano /opt/etc/init.d/S91youtubeUnblock
```
Находим в тексте такой код:
```
_iptables()
{
	ARG="$@"	
	CMD=$1 # iptables or ip6tables
	ACTION=$2 # -I, -A, -D
	RULE=${@:3}  

	$CMD -C $RULE 2>/dev/null
	exists=$(( ! $? ))

	if [[ $ACTION == "-A" ]] || [[ $ACTION == "-I" ]]
	then
		if [ $exists -eq 0 ]; then
			$ARG || exit 1
		fi
	else # -D
		if [ $exists -ne 0 ]; then
			$ARG
		fi
	fi
}
```
Заменяем его на
```
_iptables()
{
	ARG="$@"	
	CMD=$1 # iptables or ip6tables
	ACTION=$2 # -I, -A, -D
        shift; shift;
	RULE="$@"

	$CMD -C $RULE 2>/dev/null

	exists=$(( ! $? ))

	if [[ $ACTION == "-A" ]] || [[ $ACTION == "-I" ]]
	then
		if [ $exists -eq 0 ]; then
			$ARG || exit 1
		fi
	else # -D
		if [ $exists -ne 0 ]; then
			$ARG
		fi
	fi
}
```
Жмем для выходя из редактирования Ctrl+X, соглашаемся сохранить файл Y, подтверждаем то же имя Enter
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
