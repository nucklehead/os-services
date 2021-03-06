FROM debian:buster

# FROM amd64=debian:stretch arm64=arm64v8/debian:stretch

RUN export DEBIAN_FRONTEND=noninteractive && \
    \
    # Instalando o ca-certificates e adicionando as chaves
    apt update && \
    apt -y -q install ca-certificates wget gnupg1 && \
    wget http://repo.antixlinux.com/buster/pool/main/a/antix-archive-keyring/antix-archive-keyring_20019.3.13_all.deb -O /antix-archive-keyring.deb && \
    dpkg -i /antix-archive-keyring.deb && \
    rm -f /antix-archive-keyring.deb && \
    \
    # Adicionando repositorios do antiX Linux
    echo "deb https://mxlinux.c3sl.ufpr.br/mx-workspace/antix/buster/ buster main nosystemd nonfree" > /etc/apt/sources.list && \
    \
    # Adicionando repositorios do Devuan
    wget https://devuan.org/os/keyring/repository@devuan.org.asc -O - | apt-key add - && \
    wget http://deb.devuan.org/devuan/pool/main/d/devuan-keyring/devuan-keyring_2017.10.03_all.deb -O /keyring.deb && \
    dpkg -i /keyring.deb && \
    rm -r /keyring.deb && \ 
    echo "deb http://deb.devuan.org/merged beowulf main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://deb.devuan.org/merged beowulf-updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://deb.devuan.org/merged beowulf-security main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://deb.devuan.org/merged beowulf-proposed-updates main contrib non-free" >> /etc/apt/sources.list && \
    \
    # Adicionando arquitetura 32 bits
    dpkg --add-architecture i386 && \
    \
    # Atualizar a lista de pacotes
    apt-get update && \
    apt -y -q upgrade && \
    \
    # Instalando alguns pacotes iniciais
    apt-get install -y iptables openssh-server rsync locales sudo vim less curl ca-certificates \
                       psmisc htop kmod iproute2 net-tools gnupg1 wget cron bridge-utils \
                       consolekit pulseaudio usbutils pciutils ntp chromium g++ audacity flatpak \
                       chromium-l10n iputils-ping nload nmap whois netcat pavucontrol vlc etherwake \
                       x2goclient ffmpeg curl dialog gdb pulseaudio-equalizer qasmixer qashctl \
                       qasconfig wine32 winetricks git cmake rtl-sdr dirmngr libelf-dev xterm \
                       virt-manager qemu unrar lzop bzip2 libvirt0 libvirt-daemon-system dnsutils \
                       qemu qemu-kvm qemu-utils gvfs-backends nomacs nomacs-l10n openresolv cups \ 
                       apt-transport-https fonts-noto fonts-roboto-hinted gdebi apt-utils libglib2.0-bin \
                       xscreensaver-screensaver-bsod arp-scan speedtest-cli tcpdump traceroute iperf \
                       iptraf telnet wireshark aptitude aptitude-robot rsyslog hunspell hunspell-pt-br \
                       aspell-pt-br libreoffice-lightproof-pt-br galculator disk-manager geoclue-2.0 \
                       fbcat lm-sensors terminator sox libsox-fmt-all alsa-utils paprefs prevent-systemd && \
    \
    # Instalando o lightdm
    apt -y -q install --no-install-recommends lightdm lightdm-gtk-greeter-settings && \
    \
    # Alguns aplicativos do Gnome
    apt -y -q install --no-install-recommends network-manager-gnome gparted gnome-calculator gnome-disk-utility gnome-software-plugin-flatpak && \
    \
    # Alguns aplicativos do MATE
    apt -y -q install --no-install-recommends mate-system-monitor engrampa pluma && \
    \
    # Alguns aplicativos do KDE
    apt -y -q install --no-install-recommends okular kcolorchooser && \
    \
    # Instalando o LibreOffice e compilador de Go
    apt -y -q install libreoffice libreoffice-l10n-pt-br libreoffice-help-pt-br libreoffice-officebean libreoffice-avmedia-backend-vlc libreoffice-gtk3 libreoffice-pdfimport libreoffice-subsequentcheckbase golang hplip hplip-data && \
    \
    # Adicionando chaves de repositorios
    # wget -O - https://download.videolan.org/pub/debian/videolan-apt.asc | apt-key add - && \
    curl -sS https://download.spotify.com/debian/pubkey.gpg | apt-key add - && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 931FF8E79F0876134EDDBDCCA87FF9DF48BF1C90 && \
    wget -O - https://download.opensuse.org/repositories/shells:fish:release:3/Debian_9.0/Release.key | apt-key add - && \
    wget -q "http://deb.playonlinux.com/public.gpg" -O- | apt-key add - && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 73C3DB2A && \
    \
    # Adicionando repositorios do Spotify, Video Lan Center e FISH
    # echo "deb https://download.videolan.org/pub/debian/stable/ /" > /etc/apt/sources.list.d/video-lan-center.list && \
    echo "deb http://repository.spotify.com stable non-free" > /etc/apt/sources.list.d/spotify.list && \
    echo 'deb http://download.opensuse.org/repositories/shells:/fish:/release:/3/Debian_9.0/ /' > /etc/apt/sources.list.d/shells:fish:release:3.list && \
    wget http://deb.playonlinux.com/playonlinux_stretch.list -O /etc/apt/sources.list.d/playonlinux.list && \
    echo "deb http://ppa.launchpad.net/linuxuprising/java/ubuntu bionic main" >> /etc/apt/sources.list.d/java-11-oracle.list && \
    apt update && \
    \
    # Instalando pacotes da interface grafica em si e a biblioteca de DVD libdvdcss2
    apt -y -q install spotify-client xfce4 xfce4-goodies xfce4-screenshooter xfce4-notes-plugin xfce4-terminal xfce4-datetime-plugin xfce4-whiskermenu-plugin libappindicator1 albatross-gtk-theme arc-theme xscreensaver libdvd-pkg breeze-icon-theme breeze-cursor-theme fish thunar-dropbox-plugin playonlinux && \
    \
    # Ativando a biblioteca libdvdnss
    dpkg-reconfigure debconf -f noninteractive libdvd-pkg && \
    \
    # Adicionar repositorio flathub ao flatpak
    flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo && \
    \
    # Instalando o tema Adapta
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
    # Instalar o Webmin
    rm /etc/apt/apt.conf.d/docker-gzip-indexes && \
    rm /var/lib/apt/lists/*lz4 && \
    apt-get -o Acquire::GzipIndexes=false update && \
    apt -y -q install libauthen-pam-perl apt-show-versions && \
    wget https://ufpr.dl.sourceforge.net/project/webadmin/webmin/1.930/webmin_1.930_all.deb -O /webmin.deb && \
    dpkg -i /webmin.deb && \
    rm -r /webmin.deb && \
    apt update && \
    \
    # Configurar o VIM
    echo -n "set number\nsy on\nset paste\nset background=dark" > /root.vimrc && \
    \
    # Desktop do Equalizador do Pulseaudio
    echo "[Desktop Entry]" > /usr/share/applications/pulseaudio-equalizer.desktop && \
    echo "Version=1.0" >> /usr/share/applications/pulseaudio-equalizer.desktop && \
    echo "Encoding=UTF-8" >> /usr/share/applications/pulseaudio-equalizer.desktop && \
    echo "Name=PulseAudio Equalizer" >> /usr/share/applications/pulseaudio-equalizer.desktop && \
    echo "GenericName=Equalizer" >> /usr/share/applications/pulseaudio-equalizer.desktop && \
    echo "Comment=PulseAudio" >> /usr/share/applications/pulseaudio-equalizer.desktop && \
    echo "Exec=qpaeq" >> /usr/share/applications/pulseaudio-equalizer.desktop && \
    echo "Icon=multimedia-volume-control" >> /usr/share/applications/pulseaudio-equalizer.desktop && \
    echo "StartupNotify=true" >> /usr/share/applications/pulseaudio-equalizer.desktop && \
    echo "Type=Application" >> /usr/share/applications/pulseaudio-equalizer.desktop && \
    echo "Categories=AudioVideo;Audio;Mixer;GTK;" >> /usr/share/applications/pulseaudio-equalizer.desktop && \
    \
    # Criando terminalrc no skel
    mkdir -p /etc/skel/.config/xfce4/terminal/ && \
    echo "[Configuration]" > /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "FontName=NotoMono Nerd Font 12" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscAlwaysShowTabs=FALSE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscBell=FALSE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscBellUrgent=FALSE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscBordersDefault=TRUE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscCursorBlinks=FALSE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscCursorShape=TERMINAL_CURSOR_SHAPE_BLOCK" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscDefaultGeometry=80x24" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscInheritGeometry=FALSE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscMenubarDefault=TRUE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscMouseAutohide=FALSE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscMouseWheelZoom=TRUE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscToolbarDefault=FALSE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscConfirmClose=TRUE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscCycleTabs=TRUE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscTabCloseButtons=TRUE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscTabCloseMiddleClick=TRUE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscTabPosition=GTK_POS_TOP" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscHighlightUrls=TRUE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscMiddleClickOpensUri=FALSE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscCopyOnSelect=FALSE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscDefaultWorkingDir=" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscRewrapOnResize=TRUE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscUseShiftArrowsToScroll=FALSE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "MiscSlimTabs=FALSE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "ScrollingUnlimited=TRUE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    echo "ScrollingOnOutput=FALSE" >> /etc/skel/.config/xfce4/terminal/terminalrc && \
    \
    # Criar script que instala o Java 8 da Oracle. Meu servidor vai acabar um dia!!!!!!!!!
    echo '#!/bin/bash' > /usr/local/bin/instalar-java-8-oracle && \
    echo "" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "apt update && apt -y -q install java-common" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "wget https://jpmsb.sj.ifsc.edu.br/java8.tar.gz -O /java8.tar.gz" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "mkdir -p /usr/lib/jvm/" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "tar -xvf /java8.tar.gz -C /usr/lib/jvm/" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "rm -r /java8.tar.gz" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/ControlPanel ControlPanel /usr/lib/jvm/java-8-oracle/jre/bin/ControlPanel 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/java java /usr/lib/jvm/java-8-oracle/jre/bin/java 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/javaws javaws /usr/lib/jvm/java-8-oracle/jre/bin/javaws 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jcontrol jcontrol /usr/lib/jvm/java-8-oracle/jre/bin/jcontrol 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jjs jjs /usr/lib/jvm/java-8-oracle/jre/bin/jjs 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/keytool keytool /usr/lib/jvm/java-8-oracle/jre/bin/keytool 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/orbd orbd /usr/lib/jvm/java-8-oracle/jre/bin/orbd 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/pack200 pack200 /usr/lib/jvm/java-8-oracle/jre/bin/pack200 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/policytool policytool /usr/lib/jvm/java-8-oracle/jre/bin/policytool 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/rmid rmid /usr/lib/jvm/java-8-oracle/jre/bin/rmid 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/rmiregistry rmiregistry /usr/lib/jvm/java-8-oracle/jre/bin/rmiregistry 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/servertool servertool /usr/lib/jvm/java-8-oracle/jre/bin/servertool 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/tnameserv tnameserv /usr/lib/jvm/java-8-oracle/jre/bin/tnameserv 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/unpack200 unpack200 /usr/lib/jvm/java-8-oracle/jre/bin/unpack200 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/lib/jexec jexec /usr/lib/jvm/java-8-oracle/jre/lib/jexec 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/appletviewer appletviewer /usr/lib/jvm/java-8-oracle/bin/appletviewer 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/extcheck extcheck /usr/lib/jvm/java-8-oracle/bin/extcheck 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/idlj idlj /usr/lib/jvm/java-8-oracle/bin/idlj 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jar jar /usr/lib/jvm/java-8-oracle/bin/jar 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jarsigner jarsigner /usr/lib/jvm/java-8-oracle/bin/jarsigner 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/java-8-oracle/bin/javac 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/javadoc javadoc /usr/lib/jvm/java-8-oracle/bin/javadoc 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/javafxpackager javafxpackager /usr/lib/jvm/java-8-oracle/bin/javafxpackager 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/javah javah /usr/lib/jvm/java-8-oracle/bin/javah 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/javap javap /usr/lib/jvm/java-8-oracle/bin/javap 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/javapackager javapackager /usr/lib/jvm/java-8-oracle/bin/javapackager 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jcmd jcmd /usr/lib/jvm/java-8-oracle/bin/jcmd 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jconsole jconsole /usr/lib/jvm/java-8-oracle/bin/jconsole 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jdb jdb /usr/lib/jvm/java-8-oracle/bin/jdb 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jdeps jdeps /usr/lib/jvm/java-8-oracle/bin/jdeps 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jhat jhat /usr/lib/jvm/java-8-oracle/bin/jhat 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jinfo jinfo /usr/lib/jvm/java-8-oracle/bin/jinfo 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jmap jmap /usr/lib/jvm/java-8-oracle/bin/jmap 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jmc jmc /usr/lib/jvm/java-8-oracle/bin/jmc 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jps jps /usr/lib/jvm/java-8-oracle/bin/jps 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jrunscript jrunscript /usr/lib/jvm/java-8-oracle/bin/jrunscript 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jsadebugd jsadebugd /usr/lib/jvm/java-8-oracle/bin/jsadebugd 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jstack jstack /usr/lib/jvm/java-8-oracle/bin/jstack 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jstat jstat /usr/lib/jvm/java-8-oracle/bin/jstat 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jstatd jstatd /usr/lib/jvm/java-8-oracle/bin/jstatd 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/jvisualvm jvisualvm /usr/lib/jvm/java-8-oracle/bin/jvisualvm 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/native2ascii native2ascii /usr/lib/jvm/java-8-oracle/bin/native2ascii 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/rmic rmic /usr/lib/jvm/java-8-oracle/bin/rmic 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/schemagen schemagen /usr/lib/jvm/java-8-oracle/bin/schemagen 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/serialver serialver /usr/lib/jvm/java-8-oracle/bin/serialver 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/wsgen wsgen /usr/lib/jvm/java-8-oracle/bin/wsgen 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/wsimport wsimport /usr/lib/jvm/java-8-oracle/bin/wsimport 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-alternatives --install /usr/bin/xjc xjc /usr/lib/jvm/java-8-oracle/bin/xjc 180" >> /usr/local/bin/instalar-java-8-oracle && \
    echo "update-java-alternatives -s java-8-oracle" >> /usr/local/bin/instalar-java-8-oracle && \
    \
    # Definindo o layout de teclado para ABNT 2
    mkdir -p /etc/skel/.config/xfce4/xfconf/xfce-perchannel-xml/ && \
    echo '<?xml version="1.0" encoding="UTF-8"?>' > /etc/skel/.config/xfce4/xfconf/xfce-perchannel-xml/keyboard-layout.xml && \
    echo '' >> /etc/skel/.config/xfce4/xfconf/xfce-perchannel-xml/keyboard-layout.xml && \
    echo '<channel name="keyboard-layout" version="1.0">' >> /etc/skel/.config/xfce4/xfconf/xfce-perchannel-xml/keyboard-layout.xml && \
    echo '  <property name="Default" type="empty">' >> /etc/skel/.config/xfce4/xfconf/xfce-perchannel-xml/keyboard-layout.xml && \
    echo '    <property name="XkbDisable" type="bool" value="false"/>' >> /etc/skel/.config/xfce4/xfconf/xfce-perchannel-xml/keyboard-layout.xml && \
    echo '    <property name="XkbLayout" type="string" value="br"/>' >> /etc/skel/.config/xfce4/xfconf/xfce-perchannel-xml/keyboard-layout.xml && \
    echo '    <property name="XkbVariant" type="string" value=""/>' >> /etc/skel/.config/xfce4/xfconf/xfce-perchannel-xml/keyboard-layout.xml && \
    echo '  </property>' >> /etc/skel/.config/xfce4/xfconf/xfce-perchannel-xml/keyboard-layout.xml && \
    echo '</channel>' >> /etc/skel/.config/xfce4/xfconf/xfce-perchannel-xml/keyboard-layout.xml && \
    \
    # Remover o pacote light-locker para previnir que a tela deixe de ser exibida ao retornar no bloqueio do lightdm
    apt -y -q purge light-locker && \
    \
    # Fazendo uma limpeza
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/log/* /root/.bash_history && \
    rm -rf /etc/ssh/*key* && \
    \
    # Gambiarra para o ps
    ln -s /bin/ps /usr/bin/ps && \
    \
    # Carregar modulos para o Equalizador do Pulseaudio
    echo "" >> /etc/pulse/default.pa && \
    echo "load-module module-equalizer-sink" >> /etc/pulse/default.pa && \
    echo "load-module module-dbus-protocol" >> /etc/pulse/default.pa && \
    \
    # Truque do UDEV para impedir nomes longos em adaptadores Wi-Fi
    ln -s /dev/null /etc/udev/rules.d/80-net-setup-link.rules && \
    \
    # Criando usuarios
    addgroup --gid 1100 rancher && \
    addgroup --gid 1101 docker && \
    useradd -u 1100 -g rancher -G docker,sudo,plugdev,dialout,audio,video,netdev -c "Rancher,,," -m -s /bin/bash rancher && \
    useradd -u 1101 -g docker -G docker,sudo -c "Docker,,," -m -s /bin/bash docker && \
    echo ClientAliveInterval 180 >> /etc/ssh/sshd_config && \
    echo '## allow password less for rancher user' >> /etc/sudoers && \
    echo 'rancher ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers && \
    echo '## allow password less for docker user' >> /etc/sudoers && \
    echo 'docker ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers && \
    \
    # Ajustes no libvirt, adduser, autostart do nm-applet
    sed -i '105s/.*/greeter-hide-users=false/' /etc/lightdm/lightdm.conf && \
    sed -i '85s/.*/unix_sock_group = "rancher"/' /etc/libvirt/libvirtd.conf && \
    sed -i '102s/.*/unix_sock_rw_perms = "0770"/' /etc/libvirt/libvirtd.conf && \
    sed -i '6s:.*:DSHELL=/usr/bin/fish:' /etc/adduser.conf && \
    sed -i '77s:.*:EXTRA_GROUPS="dialout cdrom audio video plugdev netdev":' /etc/adduser.conf && \
    sed -i '81s:.*:ADD_EXTRA_GROUPS=1:' /etc/adduser.conf && \
    sed -i 's.; default-sample-format = s16le.default-sample-format = float32le.g' /etc/pulse/daemon.conf && \
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
    # Adicionando a logo do RancherOS quando se faz SSH
    echo -n "#!/bin/bash\n\ncat << \EOF\n               ,        , ______                 _                 _____ _____TM\n  ,------------|'------'| | ___ \               | |               /  _  /  ___|\n / .           '-'    |-  | |_/ /__ _ _ __   ___| |__   ___ _ __  | | | \ '--.\n \/|             |    |   |    // _' | '_ \ / __| '_ \ / _ \ '__' | | | |'--. \ \n   |   .________.'----'   | |\ \ (_| | | | | (__| | | |  __/ |    | \_/ /\__/ /\n   |   |        |   |     \_| \_\__,_|_| |_|\___|_| |_|\___|_|     \___/\____/\nEOF\n\ncat << EOF\n   \___/        \___/     Linux \$(uname -r)\n\n         RancherOS \$(uname -v) \$(hostname)\nEOF" > /etc/update-motd.d/00-header && \
    chmod +x /etc/update-motd.d/00-header && \
    \
    # Adicionando a logo do RancherOS quando executa um novo BASH
    echo -n "\n/etc/update-motd.d/00-header" >> /etc/bash.bashrc && \
    \
    # Adicionando a logo do RancherOS quando executa um novo FISH
    echo -n "\nfunction fish_greeting\n    bash /etc/update-motd.d/00-header\nend" >> /etc/fish/config.fish && \
    \
    # Criando script que toca em /dev/sr0
    echo -n "#!/bin/bash\n\nwhile (true);do\n    touch /dev/sr0\n    sleep 1\ndone" > /usr/local/bin/tocar-sr0 && \
    chmod 100 /usr/local/bin/tocar-sr0 && \
    \
    # Desabilitando o travamento do CD pelo kernel
    echo "sys.dev.cdrom.lock=0" >> /etc/sysctl.conf && \
    \
    # Zerando a variavel DEBIAN_FRONTEND
    unset DEBIAN_FRONTEND && \
    \
    # Script que inicia todo o conjunto necessario para o bom funcionamento do console
    echo "#!/bin/bash" > /usr/local/bin/iniciar-tudo && \
    echo "" >> /usr/local/bin/iniciar-tudo && \
    echo "mount -o remount,size=1G /dev/shm" >> /usr/local/bin/iniciar-tudo && \
    echo "rm -r /tmp/.X0-lock /tmp/.X11-unix" >> /usr/local/bin/iniciar-tudo && \
    echo "/usr/local/bin/tocar-sr0 &" >> /usr/local/bin/iniciar-tudo && \
    echo "service dbus start" >> /usr/local/bin/iniciar-tudo && \
    echo "service udev start" >> /usr/local/bin/iniciar-tudo && \
    echo "service elogind start" >> /usr/local/bin/iniciar-tudo && \
    echo 'system-docker exec udev bash -c "udevadm control --reload-rules && udevadm trigger"' >> /usr/local/bin/iniciar-tudo && \
    echo "service rsyslog start" >> /usr/local/bin/iniciar-tudo && \
    echo "service libvirtd start" >> /usr/local/bin/iniciar-tudo && \
    echo "service libvirt-guests start" >> /usr/local/bin/iniciar-tudo && \
    echo "service virtlogd start" >> /usr/local/bin/iniciar-tudo && \
    echo "service webmin start" >> /usr/local/bin/iniciar-tudo && \
    echo "service network-manager start" >> /usr/local/bin/iniciar-tudo && \
    echo "service cups start" >> /usr/local/bin/iniciar-tudo && \
    echo "service cups-browsed start" >> /usr/local/bin/iniciar-tudo && \
    echo "modprobe -r evbug" >> /usr/local/bin/iniciar-tudo && \
    echo 'echo "0 2147483647" > /proc/sys/net/ipv4/ping_group_range' >> /usr/local/bin/iniciar-tudo && \
    echo "cron" >> /usr/local/bin/iniciar-tudo && \
    echo "ntpd" >> /usr/local/bin/iniciar-tudo && \
    echo 'sleep 2 && bash -c "/etc/init.d/lightdm start" &' >> /usr/local/bin/iniciar-tudo && \
    echo "ros console-init" >> /usr/local/bin/iniciar-tudo && \
    \
    # Alterando as permissoes do script de inicio
    chmod 100 /usr/local/bin/iniciar-tudo

ENTRYPOINT ["/usr/bin/ros", "entrypoint"]
