FROM debian:stretch

# FROM amd64=debian:stretch arm64=arm64v8/debian:stretch

RUN export DEBIAN_FRONTEND=noninteractive && \
    \
    # Adicionando repositorios da RNP por ser mais veloz
    echo "deb http://debian.pop-sc.rnp.br/debian stretch main contrib non-free" > /etc/apt/sources.list && \
    echo "deb http://debian.pop-sc.rnp.br/debian stretch-updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://security.debian.org stretch/updates main contrib non-free" >> /etc/apt/sources.list && \
    apt-get update && \
    apt -y -q upgrade && \
    \
    # Instalando alguns pacotes iniciais
    apt-get install -y  iptables openssh-server rsync locales sudo vim less curl ca-certificates \
                        psmisc htop kmod iproute2 net-tools gnupg1 wget cron bridge-utils \
                        consolekit pulseaudio usbutils pciutils ntp mate-terminal chromium \
                        chromium-l10n iputils-ping lightdm nload nmap whois netcat pavucontrol vlc \
                        x2goclient ffmpeg curl dialog gdb pulseaudio-equalizer qasmixer qashctl \
                        qasconfig playonlinux wine winetricks git cmake rtl-sdr eom dirmngr okular \
                        virt-manager qemu unrar lzop bzip2 galculator libvirt0 libvirt-daemon-system \
                        qemu qemu-kvm qemu-utils engrampa gnome-disk-utility \ 
                        apt-transport-https fonts-noto fonts-roboto-hinted gdebi apt-utils libglib2.0-bin \
                        xscreensaver-screensaver-bsod arp-scan speedtest-cli tcpdump traceroute iperf \
                        iptraf telnet wireshark aptitude aptitude-robot rsyslog hunspell hunspell-pt-br \
                        aspell-pt-br libreoffice-lightproof-pt-br network-manager-gnome gparted \
                        dconf-tools fbcat lm-sensors && \
    \
    # Adicionando Backports para o LibreOffice
    echo "deb http://debian.pop-sc.rnp.br/debian stretch-backports main contrib non-free" > /etc/apt/sources.list.d/stretch-backports.list && \
    apt update && \
    \
    # Instalando o LibreOffice
    apt -y -q install -t stretch-backports libreoffice libreoffice-l10n-pt-br libreoffice-help-pt-br libreoffice-officebean libreoffice-avmedia-backend-vlc libreoffice-gtk3 libreoffice-pdfimport libreoffice-subsequentcheckbase && \
    \
    # Comentando Backport
    echo "# deb http://debian.pop-sc.rnp.br/debian stretch-backports main contrib non-free" > /etc/apt/sources.list.d/stretch-backports.list && \
    \
    # Adicionando chaves de repositorios
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 931FF8E79F0876134EDDBDCCA87FF9DF48BF1C90 && \
    wget -O - https://download.videolan.org/pub/debian/videolan-apt.asc | apt-key add - && \
    wget -O - https://download.opensuse.org/repositories/shells:fish:release:3/Debian_9.0/Release.key | apt-key add - && \
    \
    # Adicionando repositorios do Spotify, Video Lan Center e FISH
    echo "deb https://download.videolan.org/pub/debian/stable/ /" > /etc/apt/sources.list.d/video-lan-center.list && \
    echo "deb http://repository.spotify.com stable non-free" > /etc/apt/sources.list.d/spotify.list && \
    echo 'deb http://download.opensuse.org/repositories/shells:/fish:/release:/3/Debian_9.0/ /' > /etc/apt/sources.list.d/shells:fish:release:3.list && \
    apt update && \
    \
    # Instalando pacotes da interface grafica em si e a biblioteca de DVD libdvdcss2
    apt -y -q install spotify-client cinnamon task-cinnamon-desktop libappindicator1 albatross-gtk-theme arc-theme xscreensaver libdvdcss2 breeze-icon-theme breeze-cursor-theme fish && \
    \
    # Instalando o tema Adpta
    wget jpmsb.sj.ifsc.edu.br/adapta.tar.xz -O /adapta.tar.xz && \
    tar -xvf /adapta.tar.xz -C /usr/share/themes/ && \
    rm -r /adapta.tar.xz && \
    \
    # Fonte Open Sans
    wget "https://fonts.google.com/download?family=Open%20Sans|Open%20Sans%20Condensed" -O open-sans.zip && \
    unzip open-sans.zip -d /usr/share/fonts/opentype/ && \
    rm -f open-sans.zip && \
    \
    # Fonte Noto com Nerd Fonts
    mkdir /usr/share/fonts/Nerd-Fonts/ && \
    wget https://jpmsb.sj.ifsc.edu.br/Noto.zip -O /Noto.zip && \
    unzip /Noto.zip -d / && \
    cp -r /Noto /usr/share/fonts/Nerd-Fonts/ && \
    rm -r /Noto.zip /Noto && \
    fc-cache -f -v && \
    \
    # Fazendo uma limpeza
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/log/* /root/.bash_history && \
    rm -rf /etc/ssh/*key* && \
    \
    # Gambiarra para o ps
    ln -s /bin/ps /usr/bin/ps && \
    \
    # Criando usuarios
    usermod -s /usr/bin/fish root && \
    addgroup --gid 1100 rancher && \
    addgroup --gid 1101 docker && \
    useradd -u 1100 -g rancher -G docker,sudo,plugdev,dialout,audio,video,netdev -c "Rancher,,," -m -s /usr/bin/fish rancher && \
    useradd -u 1101 -g docker -G docker,sudo -c "Docker,,," -m -s /bin/bash docker && \
    echo ClientAliveInterval 180 >> /etc/ssh/sshd_config && \
    echo '## allow password less for rancher user' >> /etc/sudoers && \
    echo 'rancher ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers && \
    echo '## allow password less for docker user' >> /etc/sudoers && \
    echo 'docker ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers && \
    echo '## allow password less execution of nm-applet as a privileged user' >> /etc/sudoers && \
    echo 'ALL ALL=(ALL) NOPASSWD: /usr/bin/nm-applet' >> /etc/sudoers && \
    \
    # Ajustes no libvirt, adduser, autostart do nm-applet
    sed -i '105s/.*/greeter-hide-users=false/' /etc/lightdm/lightdm.conf && \
    sed -i '22s/.*/greeter-user=root/' /etc/lightdm/lightdm.conf && \
    sed -i '85s/.*/unix_sock_group = "rancher"/' /etc/libvirt/libvirtd.conf && \
    sed -i '102s/.*/unix_sock_rw_perms = "0770"/' /etc/libvirt/libvirtd.conf && \
    sed -i '6s:.*:DSHELL=/usr/bin/fish:' /etc/adduser.conf && \
    sed -i '77s:.*:EXTRA_GROUPS="dialout cdrom audio video plugdev netdev":' /etc/adduser.conf && \
    sed -i '81s:.*:ADD_EXTRA_GROUPS=1:' /etc/adduser.conf && \
    sed -i '143s:.*:Exec=sh -c "sudo nm-applet":' /etc/xdg/autostart/nm-applet.desktop && \
    \
    # Regras para o Network-Manager
    echo "[nm-applet]" > /etc/polkit-1/localauthority/10-vendor.d/org.freedesktop.NetworkManager.pkla && \
    echo "Identity=unix-group:netdev" >> /etc/polkit-1/localauthority/10-vendor.d/org.freedesktop.NetworkManager.pkla && \
    echo "Action=org.freedesktop.NetworkManager.enable-disable-network;org.freedesktop.NetworkManager.enable-disable-wifi;org.freedesktop.NetworkManager.enable-disable-wwan;org.freedesktop.NetworkManager.enable-disable-wimax;org.freedesktop.NetworkManager.network-control;org.freedesktop.NetworkManager.wifi.share.protected;org.freedesktop.NetworkManager.wifi.share.open;org.freedesktop.NetworkManager.settings.modify.own;org.freedesktop.NetworkManager.enable-disable-statistics" >> /etc/polkit-1/localauthority/10-vendor.d/org.freedesktop.NetworkManager.pkla && \
    echo "ResultActive=yes" >> /etc/polkit-1/localauthority/10-vendor.d/org.freedesktop.NetworkManager.pkla && \
    echo "ResultAny=yes" >> /etc/polkit-1/localauthority/10-vendor.d/org.freedesktop.NetworkManager.pkla && \
    echo "ResultInactive=no" >> /etc/polkit-1/localauthority/10-vendor.d/org.freedesktop.NetworkManager.pkla && \
    \
    # Regras para montagem
    echo "[Mount a system-internal device]" > /etc/polkit-1/localauthority/50-local.d/50-mount.pkla && \
    echo "Identity=unix-group:plugdev" >> /etc/polkit-1/localauthority/50-local.d/50-mount.pkla && \
    echo "Action=org.freedesktop.udisks2.filesystem-mount;org.freedesktop.udisks2.encrypted-unlock;org.freedesktop.udisks2.encrypted-change-passphrase;org.freedesktop.udisks2.loop-setup;org.freedesktop.udisks2.power-off-drive;org.freedesktop.udisks2.eject-media;org.freedesktop.udisks2.modify-device;org.freedesktop.udisks2.rescan;org.freedesktop.udisks2.ata-smart-update;org.freedesktop.udisks2.ata-check-power;org.freedesktop.udisks2.ata-standby;org.freedesktop.udisks2.cancel-job" >> /etc/polkit-1/localauthority/50-local.d/50-mount.pkla && \
    echo "ResultAny=yes" >> /etc/polkit-1/localauthority/50-local.d/50-mount.pkla && \
    echo "ResultActive=yes" >> /etc/polkit-1/localauthority/50-local.d/50-mount.pkla && \
    echo "ResultInactive=no" >> /etc/polkit-1/localauthority/50-local.d/50-mount.pkla && \ 
    \
    # Regra para ajuste de data e hora
    echo "[Data e hora]" > /etc/polkit-1/localauthority/50-local.d/20-org.cinnamon.settingsdaemon.datetimemechanism.pkla && \
    echo "Identity=unix-group:*" >> /etc/polkit-1/localauthority/50-local.d/20-org.cinnamon.settingsdaemon.datetimemechanism.pkla && \
    echo "Action=org.cinnamon.settingsdaemon.datetimemechanism.configure" >> /etc/polkit-1/localauthority/50-local.d/20-org.cinnamon.settingsdaemon.datetimemechanism.pkla && \
    echo "ResultActive=yes" >> /etc/polkit-1/localauthority/50-local.d/20-org.cinnamon.settingsdaemon.datetimemechanism.pkla && \
    echo "ResultAny=yes" >> /etc/polkit-1/localauthority/50-local.d/20-org.cinnamon.settingsdaemon.datetimemechanism.pkla && \
    echo "ResultInactive=no" >> /etc/polkit-1/localauthority/50-local.d/20-org.cinnamon.settingsdaemon.datetimemechanism.pkla && \
    \
    # Regra para ajustar o brilho da tela
    echo "[Cinnamon Backlight]" > /etc/polkit-1/localauthority/50-local.d/21-org.cinnamon.settings-daemon.plugins.power.pkla && \
    echo "Identity=unix-group:*" >> /etc/polkit-1/localauthority/50-local.d/21-org.cinnamon.settings-daemon.plugins.power.pkla && \
    echo "Action=org.cinnamon.settings-daemon.plugins.power.backlight-helper" >> /etc/polkit-1/localauthority/50-local.d/21-org.cinnamon.settings-daemon.plugins.power.pkla && \
    echo "ResultActive=yes" >> /etc/polkit-1/localauthority/50-local.d/21-org.cinnamon.settings-daemon.plugins.power.pkla && \
    echo "ResultAny=yes" >> /etc/polkit-1/localauthority/50-local.d/21-org.cinnamon.settings-daemon.plugins.power.pkla && \
    echo "ResultInactive=no" >> /etc/polkit-1/localauthority/50-local.d/21-org.cinnamon.settings-daemon.plugins.power.pkla && \
    \
    # Regra para ajuste de brilho da tela para Intel
    echo "[Intel backlight]" > /etc/polkit-1/localauthority/50-local.d/22-org.x.xf86-video-intel.backlight-helper.pkla && \
    echo "Identity=unix-group:*" >> /etc/polkit-1/localauthority/50-local.d/22-org.x.xf86-video-intel.backlight-helper.pkla && \
    echo "Action=org.x.xf86-video-intel.backlight-helper" >> /etc/polkit-1/localauthority/50-local.d/22-org.x.xf86-video-intel.backlight-helper.pkla && \
    echo "ResultActive=yes" >> /etc/polkit-1/localauthority/50-local.d/22-org.x.xf86-video-intel.backlight-helper.pkla && \
    echo "ResultAny=yes" >> /etc/polkit-1/localauthority/50-local.d/22-org.x.xf86-video-intel.backlight-helper.pkla && \
    echo "ResultInactive=no" >> /etc/polkit-1/localauthority/50-local.d/22-org.x.xf86-video-intel.backlight-helper.pkla && \
    \
    # Regra para o login1
    echo "[Login1]" > /etc/polkit-1/localauthority/50-local.d/23-org.freedesktop.login1.pkla && \
    echo "Identity=unix-group:*" >> /etc/polkit-1/localauthority/50-local.d/23-org.freedesktop.login1.pkla && \
    echo "Action=org.freedesktop.login1.inhibit-block-shutdown;org.freedesktop.login1.inhibit-block-sleep;org.freedesktop.login1.inhibit-handle-power-key;org.freedesktop.login1.inhibit-handle-suspend-key;org.freedesktop.login1.inhibit-handle-hibernate-key;org.freedesktop.login1.inhibit-handle-lid-switch;org.freedesktop.login1.power-off;org.freedesktop.login1.power-off-multiple-sessions;org.freedesktop.login1.reboot;org.freedesktop.login1.reboot-multiple-sessions;org.freedesktop.login1.suspend-multiple-sessions;org.freedesktop.login1.hibernate;org.freedesktop.login1.hibernate-multiple-sessions" >> /etc/polkit-1/localauthority/50-local.d/23-org.freedesktop.login1.pkla && \
    echo "ResultActive=yes" >> /etc/polkit-1/localauthority/50-local.d/23-org.freedesktop.login1.pkla && \
    echo "ResultAny=yes" >> /etc/polkit-1/localauthority/50-local.d/23-org.freedesktop.login1.pkla && \
    echo "ResultInactive=no" >> /etc/polkit-1/localauthority/50-local.d/23-org.freedesktop.login1.pkla && \
    \ 
    # Habilitando o toque no touchpad
    mkdir /etc/X11/xorg.conf.d/ && \
    echo 'Section "InputClass"' > /etc/X11/xorg.conf.d/40-libinput.conf && \
    echo '        Identifier "libinput touchpad catchall"' >> /etc/X11/xorg.conf.d/40-libinput.conf && \
    echo '        MatchIsTouchpad "on"' >> /etc/X11/xorg.conf.d/40-libinput.conf && \
    echo '        MatchDevicePath "/dev/input/event*"' >> /etc/X11/xorg.conf.d/40-libinput.conf && \
    echo '        Driver "libinput"' >> /etc/X11/xorg.conf.d/40-libinput.conf && \
    echo '        Option "Tapping" "on"' >> /etc/X11/xorg.conf.d/40-libinput.conf && \
    echo '        Option "HorizontalScrolling" "0"' >> /etc/X11/xorg.conf.d/40-libinput.conf && \
    echo 'EndSection' >> /etc/X11/xorg.conf.d/40-libinput.conf && \
    \
    # Ajuste no horario e idioma
    echo "America/Sao_Paulo" > /etc/timezone && \
    rm -r /etc/localtime && \
    ln -snf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata && \
    echo "pt_BR.UTF-8 UTF-8" > /etc/locale.gen && \
    echo 'LANG="pt_BR.UTF-8"' > /etc/default/locale && \
    dpkg-reconfigure -f noninteractive locales && \
    update-locale LANG=pt_BR.UTF-8 && \
    echo 'export LANG=pt_BR.UTF-8' >> /etc/profile && \
    echo 'setxkbmap br' >> /etc/profile && \
    locale-gen --purge en_US.UTF-8 && \
    bash -c "echo -e 'LANG="pt_BR.UTF-8"\nLANGUAGE="pt_BR:br"\n'" > /etc/default/locale && \
    \
    unset DEBIAN_FRONTEND && \
    \
    # Script que inicia todo o conjunto necessario para o bom funcionamento do console
    echo "#!/bin/bash" > /usr/local/bin/iniciar-tudo && \
    echo "" >> /usr/local/bin/iniciar-tudo && \
    echo "mount -o remount,size=1G /dev/shm" >> /usr/local/bin/iniciar-tudo && \
    echo "service dbus start" >> /usr/local/bin/iniciar-tudo && \
    echo "service udev start" >> /usr/local/bin/iniciar-tudo && \
    echo "service rsyslog start" >> /usr/local/bin/iniciar-tudo && \
    echo "service libvirtd start" >> /usr/local/bin/iniciar-tudo && \
    echo "service virtlogd start" >> /usr/local/bin/iniciar-tudo && \
    echo "service network-manager start" >> /usr/local/bin/iniciar-tudo && \
    echo "modprobe -r evbug" >> /usr/local/bin/iniciar-tudo && \
    echo 'echo "0 2147483647" > /proc/sys/net/ipv4/ping_group_range' >> /usr/local/bin/iniciar-tudo && \
    echo "cron" >> /usr/local/bin/iniciar-tudo && \
    echo "ntpd" >> /usr/local/bin/iniciar-tudo && \
    echo "service lightdm start &" >> /usr/local/bin/iniciar-tudo && \
    echo "ros console-init" >> /usr/local/bin/iniciar-tudo && \
    \
    # Alterando as permissoes do script de inicio
    chmod 100 /usr/local/bin/iniciar-tudo

ENTRYPOINT ["/usr/bin/ros", "entrypoint"]
