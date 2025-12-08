### IO Scheduler
---



#### Yöntem 1 - GRUB
---

##### 1. Kalıcı ayar için öncelikle cat /etc/default/grub dosyasında aşağıdaki değişiklik yapılır.
    -
    GRUB_CMDLINE_LINUX_DEFAULT="quiet rootdelay=5 net.ifnames=0 biosdevname=0"
    +
    GRUB_CMDLINE_LINUX_DEFAULT="quiet rootdelay=5 net.ifnames=0 biosdevname=0 elevator=deadline"

##### 2. Sonrasında gub update yapmak gerekir.
	Debian 9'da kullanılan komuttan çok emin değilim. Ya grub2-mkconfig komutu ya da update-grub komutu vardır sistemde.
    Eğer update grub2-mkconfig ile yapılacaksa aşağıdaki komutu çalıştırmak lazım. 
    update-grub ya da update-grub2 komutu zaten yukarıdaki komutu çalıştıran bir scripttir. Varsa bu komut çalıştırılabilir.
 
	BIOS tabanlı sistemlerde:
    grub2-mkconfig -o /boot/grub2/grub.cfg

    UEFI tabanlı sistemlerde: 
    grub2-mkconfig -o /boot/efi/EFI/debian/grub.cfg (path'ten çok emin değilim)

##### 3. Neticede reboot gerekir. Sistem açıldığında aşağıdaki komut ile disklerde io scheduler olarak ne atandığı kontrol edilebilir.
	cat /sys/block/<dm-XX>/queue/scheduler
    ya da 
    cat /proc/cmdline




#### Yöntem 2 - UDEV
--------------------
Bir önceki yöntem tüm sistemdeki disklerin io scheduler'ini deadline olarak ayarlıyor. Bu ayarı local diskleri dışarıda bırakacak şekilde yapmak daha doğru olabilir. Bunun için udev kullanılır.

##### 1. /etc/udev/rules.d/99-scheduler.rules dosyası yoksa yaratılır ve içine aşadakine benzer kurallar yazılır.
    ACTION=="add|change", KERNEL=="sd*[!0-9]", SUBSYSTEM=="block", ENV{ID_VENDOR}=="HITACHI", ATTR{queue/scheduler}="deadline"
    ACTION=="add|change", KERNEL=="dm-[0-9]*", SUBSYSTEM=="block", ENV{DM_NAME}=="360060e80*", ATTR{queue/scheduler}="deadline"

##### 2. Ardından udev kuralları yeniden yüklenir.
	udevadm control --reload-rules

##### 3. IO scheduler ayarları yüklenir:
	udevadm trigger --type=devices --action=change
	
##### 4. Yine değerler kontrol edilir:
	cat /sys/block/<dm-XX>/queue/scheduler
