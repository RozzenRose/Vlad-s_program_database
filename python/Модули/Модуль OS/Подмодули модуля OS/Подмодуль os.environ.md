#модуль #python #OS #environ


```python
import OS
```
словарь, который предоставляет доступ к переменным окружения. 

Рассмотрим словарь поближе: `(ключь, значение)`
```python
import os
print(*os.environ.items(), sep='\n')
```
```
('PATH', '/home/vladick/.local/bin:/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/var/lib/flatpak/exports/bin:/usr/lib/jvm/default/bin:/usr/bin/site_perl:/usr/bin/vendor_perl:/usr/bin/core_perl:/var/lib/snapd/snap/bin')
('LC_MEASUREMENT', 'en_US.UTF-8')
('XAUTHORITY', '/home/vladick/.Xauthority')
('LC_TELEPHONE', 'en_US.UTF-8')
('GDMSESSION', 'xfce')
('XDG_DATA_DIRS', '/home/vladick/.local/share/flatpak/exports/share:/var/lib/flatpak/exports/share:/usr/local/share:/usr/share:/var/lib/snapd/desktop:/usr/share')
('LC_TIME', 'en_US.UTF-8')
('MOTD_SHOWN', 'pam')
('DBUS_SESSION_BUS_ADDRESS', 'unix:path=/run/user/1000/bus')
('XDG_CURRENT_DESKTOP', 'XFCE')
('MAIL', '/var/spool/mail/vladick')
('SSH_AGENT_PID', '1116')
('LC_PAPER', 'en_US.UTF-8')
('SESSION_MANAGER', 'local/vladick-vmwarevirtualplatform:@/tmp/.ICE-unix/1058,unix/vladick-vmwarevirtualplatform:/tmp/.ICE-unix/1058')
('LOGNAME', 'vladick')
('PWD', '/mnt/hgfs/PyCharm_folder/Stepik_bufer')
('PYCHARM_HOSTED', '1')
('PYTHONPATH', '/mnt/hgfs/PyCharm_folder/Stepik_bufer')
('SHELL', '/bin/bash')
('LC_ADDRESS', 'en_US.UTF-8')
('PYCHARM_CLASSPATH', '/usr/share/pycharm/jbr//lib/*:/usr/lib/jvm/java-17-openjfx//lib/*')
('GTK_MODULES', 'canberra-gtk-module:canberra-gtk-module')
('PANEL_GDK_CORE_DEVICE_EVENTS', '0')
('XDG_SESSION_PATH', '/org/freedesktop/DisplayManager/Session0')
('DEBUGINFOD_URLS', 'https://debuginfod.archlinux.org ')
('XDG_SESSION_DESKTOP', 'xfce')
('SHLVL', '0')
('LC_IDENTIFICATION', 'en_US.UTF-8')
('LC_MONETARY', 'en_US.UTF-8')
('XDG_CONFIG_DIRS', '/etc/xdg')
('XDG_SEAT_PATH', '/org/freedesktop/DisplayManager/Seat0')
('LANG', 'en_US.utf8')
('XDG_SESSION_ID', '2')
('XDG_SESSION_TYPE', 'x11')
('DISPLAY', ':0.0')
('LC_NAME', 'en_US.UTF-8')
('XDG_SESSION_CLASS', 'user')
('GDM_LANG', 'en_US.utf8')
('GTK3_MODULES', 'xapp-gtk3-module:xapp-gtk3-module')
('PYTHONIOENCODING', 'UTF-8')
('XDG_GREETER_DATA_DIR', '/var/lib/lightdm-data/vladick')
('PYCHARM_JDK', '/usr/share/pycharm/jbr/')
('DESKTOP_SESSION', 'xfce')
('USER', 'vladick')
('XDG_MENU_PREFIX', 'xfce-')
('LC_NUMERIC', 'en_US.UTF-8')
('XDG_SEAT', 'seat0')
('SSH_AUTH_SOCK', '/tmp/ssh-XXXXXXnbWuYb/agent.1115')
('PYTHONUNBUFFERED', '1')
('XDG_RUNTIME_DIR', '/run/user/1000')
('XDG_VTNR', '7')
('HOME', '/home/vladick')
```
Это `os.environ` вызванные на `ArchLinux` в сборке `Monjaro`. Если вызвать то же самое не другой OS, то этот словарь будет отличаться. Подробнее об этом подмодуле можно почитать тут https://stackoverflow.com/questions/5971312/how-to-set-environment-variables-in-python