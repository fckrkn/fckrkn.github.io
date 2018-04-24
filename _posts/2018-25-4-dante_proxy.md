---
layout: post
title: Поднимаем SOCKS5 прокси в шесть шагов
---

---
<h1>Если вы планируете поднять свой сервер прокси на своем VDS/VPS, то сделать это не сильно большая проблема.</h1>
<br />В примере буду использовались Debian 9 и Dante.
<br />
<h2>1. Логинимся на наш сервер под пользователем root</h2>
<br />root@server# apt-get update
<br />root@server# apt-get install dante-server
<br />
<h2>2. Делаем резервную копию файла настройки Dante и создаем свой</h2>
<br />mv /etc/dante.conf /etc/dante.conf.bak
<br />nano /etc/dante.conf
<br />
<h2>3. Указываем следующие настройки в файле конфигурации (вместо xxx.xxx.xxx.xxx указываем IP нашего сервера)</h2>
<br />logoutput: stderr
<br />internal: xxx.xxx.xxx.xxx port = 1080
<br />external: xxx.xxx.xxx.xxx
<br />
<br />socksmethod: username
<br />user.privileged: root
<br />user.unprivileged: nobody
<br />user.libwrap: nobody
<br />
<br />client pass {
<br />    from: 0/0 to: 0/0
<br />    log: error
<br />}
<br />
<br />socks pass {
<br />    from: 0/0 to: 0/0
<br />    log: error
<br />}
<br />
<h2>4. Перезапускаем Dante</h2>
<br />sytemctl restart dante
<br />
<h2>5. Добавляем пользователя и задаем пароль</h2>
<br />root@server# useradd —shell /usr/sbin/nologin USERNAME
<br />root@server# passwd USERNAME
<br />
<h2>6. Проверяем наш прокси с локальной машины</h2>
<br />user@local-pc$ curl -x socks5://USERNAME:PASSWORD@xxx.xxx.xxx.xxx:1080 https://api.ipify.org/
<br />
<br />
<br /><a href="https://www.inet.no/dante/">Ссылка на сайт Dante</a>
<br />
<br />
<br />Написано для канала <a href="https://t.me/cultofwire">Cult of Wire</a>
 ---
