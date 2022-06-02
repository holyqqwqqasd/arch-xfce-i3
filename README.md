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

1. Установить группу пакетов **xfce4** **i3-gaps**
2. Установить пакеты оформления для xfc3: **materia-gtk-theme** **papirus-icon-theme** **apple_cursor**
3. Всякие апплеты и плагины: **network-manager-applet** **xfce4-pulseaudio-plugin** **xfce4-xkb-plugin** **xfce4-whiskermenu-plugin** **xfce4-screensaver**
4. Прочие утилиты: **ristretto** **maim** **sclip** **picom** **alacritty** **nitrogen**
5. Из аура вытянуть **xfce4-i3-workspaces-plugin-git** **system-monitoring-center**

Одной командой:
```
pacman -S xfce4 i3-gaps materia-gtk-theme papirus-icon-theme apple_cursor network-manager-applet xfce4-pulseaudio-plugin xfce4-xkb-plugin xfce4-whiskermenu-plugin xfce4-screensaver ristretto maim sclip picom alacritty nitrogen

paru -S xfce4-i3-workspaces-plugin-git system-monitoring-center
```

### Меняем оконный менеджер в xfce4 на i3wm

1. Вырубить в сессии *Settings -> Session and Startup -> Current Session* сохранение информации о **xfwm4** и **xfdesktop** (не забыть сохранить), и добавить на вкладке *Application Autostart* автозапуск **i3**
2. В *Settings -> Keyboard* удалить все шорткаты чтобы не мешалось в i3
3. Добавить на панель задач xfce4-i3-workspaces-plugin

### Конфиги для i3 picom alacritty

Все настройки i3 (автозапуск, хоткеи) в папке **config**

Создать линк на конфиг файлы
```
ln -s $PWD/config/i3/config ~/.config/i3/config &&
ln -s $PWD/config/picom.conf ~/.config/picom.conf &&
ln -s $PWD/config/alacritty/alacritty.yml ~/.config/alacritty/alacritty.yml
```

Если нужно удалить старые файлы
```
rm ~/.config/i3/config ~/.config/picom.conf ~/.config/alacritty/alacritty.yml
```
