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

## Создание бекапов:

Установить утилиту **timeshift-bin** из AUR


# Рабочее окружение Xfce4 + i3

*TODO: Установка окружения*

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
