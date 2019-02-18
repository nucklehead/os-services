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
                        consolekit pulseaudio lightdm usbutils pciutils ntp mate-terminal chromium \
                        chromium-l10n iputils-ping nload nmap whois netcat pavucontrol vlc \
                        x2goclient ffmpeg curl dialog gdb pulseaudio-equalizer qasmixer qashctl \
                        qasconfig playonlinux wine winetricks git cmake rtl-sdr eom dirmngr okular \
                        virt-manager qemu unrar lzop bzip2 galculator libvirt0 libvirt-daemon-system \
                        qemu qemu-kvm qemu-utils engrampa mate-system-monitor gnome-disk-utility \ 
                        apt-transport-https fonts-noto fonts-roboto-hinted gdebi apt-utils libglib2.0-bin \
                        xscreensaver-screensaver-bsod arp-scan speedtest-cli tcpdump traceroute iperf \
                        iptraf telnet wireshark aptitude aptitude-robot rsyslog hunspell hunspell-pt-br \
                        aspell-pt-br libreoffice-lightproof-pt-br network-manager-gnome gparted \
                        dconf-tools pluma && \
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
    \
    # Adicionando repositorios do Spotify e Video Lan Center
    echo "deb https://download.videolan.org/pub/debian/stable/ /" > /etc/apt/sources.list.d/video-lan-center.list && \
    echo "deb http://repository.spotify.com stable non-free" > /etc/apt/sources.list.d/spotify.list && \
    apt update && \
    \
    # Instalando pacotes da interface grafica em si e a biblioteca de DVD libdvdcss2
    apt -y -q install spotify-client xfce4 xfce4-goodies xfce4-screenshooter xfce4-notes-plugin xfce4-terminal xfce4-datetime-plugin xfce4-topmenu-plugin xfce4-whiskermenu-plugin libappindicator1 albatross-gtk-theme arc-theme xscreensaver libdvdcss2 breeze-icon-theme breeze-cursor-theme && \
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
    # Fazendo uma limpeza
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/log/* /root/.bash_history && \
    rm -rf /etc/ssh/*key* && \
    \
    # Criando usuarios
    addgroup --gid 1100 rancher && \
    addgroup --gid 1101 docker && \
    useradd -u 1100 -g rancher -G docker,sudo,plugdev,dialout -c "Rancher,,," -m -s /bin/bash rancher && \
    useradd -u 1101 -g docker -G docker,sudo -c "Docker,,," -m -s /bin/bash docker && \
    echo ClientAliveInterval 180 >> /etc/ssh/sshd_config && \
    echo '## allow password less for rancher user' >> /etc/sudoers && \
    echo 'rancher ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers && \
    echo '## allow password less for docker user' >> /etc/sudoers && \
    echo 'docker ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers && \
    \
    # Ajustes no lightdm e libvirt
    sed -i '105s/.*/greeter-hide-users=false/' /etc/lightdm/lightdm.conf && \
    sed -i '22s/.*/greeter-user=root/' /etc/lightdm/lightdm.conf && \
    sed -i '85s/.*/unix_sock_group = "rancher"/' /etc/libvirt/libvirtd.conf && \
    sed -i '102s/.*/unix_sock_rw_perms = "0770"/' /etc/libvirt/libvirtd.conf && \
    \
    # Ajustes no polkit para usuario normal poder manipular dispositivos
    sed -i '84s:.*:      <allow_any>yes</allow_any>:' /usr/share/polkit-1/actions/org.freedesktop.udisks2.policy && \
    sed -i '985s:.*:      <allow_any>yes</allow_any>:' /usr/share/polkit-1/actions/org.freedesktop.udisks2.policy && \
    sed -i '1340s:.*:      <allow_any>yes</allow_any>:' /usr/share/polkit-1/actions/org.freedesktop.udisks2.policy && \
    sed -i '1548s:.*:      <allow_any>yes</allow_any>:' /usr/share/polkit-1/actions/org.freedesktop.udisks2.policy && \
    sed -i '2070s:.*:      <allow_any>yes</allow_any>:' /usr/share/polkit-1/actions/org.freedesktop.udisks2.policy && \
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
