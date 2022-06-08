# Разметка диска

https://ctlos.github.io/wiki/btrfs/btrfs-part1/

```
mkfs.btrfs -L arch /dev/sda3 -f
mount /dev/sda3 /mnt
cd /mnt
btrfs subvolume create @
btrfs subvolume create @var
btrfs subvolume create @home
cd /
umount /mnt
mount -o noatime,compress=zstd,subvol=@ /dev/sda3 /mnt
mkdir -p /mnt/{home,boot/efi,var}
mount -o noatime,compress=zstd,subvol=@var /dev/sda3 /mnt/var
mount -o noatime,compress=zstd,subvol=@home /dev/sda3 /mnt/home
mount /dev/sda1 /mnt/boot/efi
swapon /dev/sda2
```

После этого не забыть убрать fsck из **/etc/mkinitcpio.conf** и выполнить команду `mkinitcpio -P`
Затем из **/etc/fstab** убрать subvolid у монтированных сабволов

### Создание бекапов:

Установить утилиту **timeshift-bin** из AUR


# Рабочее окружение Xfce4 + i3-gaps

### Установка необходимого

1. Установить группу пакетов **xfce4 i3-gaps**
2. Установить пакеты оформления для xfc3: **materia-gtk-theme papirus-icon-theme apple_cursor**
3. Всякие апплеты и плагины: **network-manager-applet xfce4-pulseaudio-plugin xfce4-xkb-plugin xfce4-whiskermenu-plugin**
4. Прочее: **ristretto maim sclip picom alacritty nitrogen *lxappearance***
5. Из аура вытянуть: **noto-fonts-emoji-apple xfce4-i3-workspaces-plugin-git**

Одной командой:
```
pacman -S xfce4 i3-gaps materia-gtk-theme papirus-icon-theme apple_cursor network-manager-applet xfce4-pulseaudio-plugin xfce4-xkb-plugin xfce4-whiskermenu-plugin ristretto maim sclip picom alacritty nitrogen

paru -S noto-fonts-emoji-apple xfce4-i3-workspaces-plugin-git
```

### Меняем оконный менеджер в xfce4 на i3wm

1. Вырубить в сессии *Settings -> Session and Startup -> Current Session* сохранение информации о **xfwm4** и **xfdesktop** т.е. установить *Never* (и не забыть сохранить!)
2. Добавить в *Settings -> Session and Startup -> Application Autostart* автозапуск **i3**
3. В *Settings -> Keyboard* удалить все шорткаты чтобы не мешалось в i3
4. Добавить на панель задач xfce4-i3-workspaces-plugin

### Локер и экран входа

1. Устанавливаем пакеты: **lightdm lightdm-webkit2-greeter lightdm-webkit-theme-litarvan**
2. В `/etc/lightdm/lightdm.conf` установить значение `greeter-session=lightdm-webkit2-greeter`
3. В `/etc/lightdm/lightdm-webkit2-greeter.conf` установить значение `webkit-theme` (или `theme`) в `litarvan`
4. Чтобы lightdm локер блокировался через xflock4, надо выполнить команду: `xfconf-query --create -c xfce4-session -p /general/LockCommand -t string -s "light-locker-command --lock"`
5. Чтобы настроить фото пользователя в lightdm, нужно поковыряться в `/var/lib/AccountsService/*`

Одной командой:
```
pacman -S lightdm lightdm-webkit2-greeter lightdm-webkit-theme-litarvan
```

### Конфиги для i3 picom alacritty

Все настройки i3 (автозапуск, хоткеи) в папке **config**

Создать линк на конфиг файлы
```
ln -s $PWD/config/i3/config ~/.config/i3/config
ln -s $PWD/config/picom/picom.conf ~/.config/picom/picom.conf
ln -s $PWD/config/alacritty/alacritty.yml ~/.config/alacritty/alacritty.yml
ln -s $PWD/config/.zshrc ~/.zshrc
```
