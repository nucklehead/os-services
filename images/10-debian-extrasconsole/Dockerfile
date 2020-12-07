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
                        psmisc htop kmod iproute2 net-tools gnupg1 wget cron bridge-utils usbutils \
                        pciutils ntp iputils-ping nload nmap whois netcat dialog dirmngr unrar bzip2 \
                        apt-transport-https apt-utils arp-scan speedtest-cli tcpdump traceroute iperf \
                        iptraf telnet aptitude aptitude-robot udev rsyslog gcc make perl libelf-dev \ 
                        lm-sensors && \
    \
    # Interpretador FISH
    wget -O - https://download.opensuse.org/repositories/shells:fish:release:3/Debian_9.0/Release.key | apt-key add - && \
    echo 'deb http://download.opensuse.org/repositories/shells:/fish:/release:/3/Debian_9.0/ /' > /etc/apt/sources.list.d/shells:fish:release:3.list && \
    apt update && \
    apt -y -q install fish && \
    \
    # Fazendo uma limpeza
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/log/* /root/.bash_history && \
    rm -rf /etc/ssh/*key* && \
    \
    # Script para verificar pacotes
    wget https://raw.githubusercontent.com/jpmsb/gui/master/scripts/pacote -O /usr/local/bin/pacote && \
    chmod 555 /usr/local/bin/pacote && \
    \
    # Gambiarra para o ps
    ln -s /bin/ps /usr/bin/ps && \
    \
    # Ajustes no adduser
    sed -i '6s:.*:DSHELL=/usr/bin/fish:' /etc/adduser.conf && \
    \
    # Criando usuarios
    usermod -s /usr/bin/fish root && \
    addgroup --gid 1100 rancher && \
    addgroup --gid 1101 docker && \
    useradd -u 1100 -g rancher -G docker,sudo,plugdev,dialout -c "Rancher,,," -m -s /usr/bin/fish rancher && \
    useradd -u 1101 -g docker -G docker,sudo -c "Docker,,," -m -s /bin/bash docker && \
    echo ClientAliveInterval 180 >> /etc/ssh/sshd_config && \
    echo '## allow password less for rancher user' >> /etc/sudoers && \
    echo 'rancher ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers && \
    echo '## allow password less for docker user' >> /etc/sudoers && \
    echo 'docker ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers && \
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
    locale-gen --purge en_US.UTF-8 && \
    bash -c "echo -e 'LANG="pt_BR.UTF-8"\nLANGUAGE="pt_BR:br"\n'" > /etc/default/locale && \
    \
    # Adicionando a logo do RancherOS quando se faz SSH
    echo -n "#!/bin/bash\n\ncat << \EOF\n               ,        , ______                 _                 _____ _____TM\n  ,------------|'------'| | ___ \               | |               /  _  /  ___|\n / .           '-'    |-  | |_/ /__ _ _ __   ___| |__   ___ _ __  | | | \ '--.\n \/|             |    |   |    // _' | '_ \ / __| '_ \ / _ \ '__' | | | |'--. \ \n   |   .________.'----'   | |\ \ (_| | | | | (__| | | |  __/ |    | \_/ /\__/ /\n   |   |        |   |     \_| \_\__,_|_| |_|\___|_| |_|\___|_|     \___/\____/\nEOF\n\ncat << EOF\n   \___/        \___/     Linux \$(uname -r)\n\n         RancherOS \$(uname -v) \$(hostname)\nEOF" > /etc/update-motd.d/00-header && \
    \
    # Zerando a variavel DEBIAN_FRONTEND
    unset DEBIAN_FRONTEND && \
    \
    # Script que inicia todo o conjunto necessario para o bom funcionamento do console
    echo "#!/bin/bash" > /usr/local/bin/iniciar-tudo && \
    echo "" >> /usr/local/bin/iniciar-tudo && \
    echo "mount -o remount,size=1G /dev/shm" >> /usr/local/bin/iniciar-tudo && \
    echo "service dbus start" >> /usr/local/bin/iniciar-tudo && \
    echo "service udev start" >> /usr/local/bin/iniciar-tudo && \
    echo "service rsyslog start" >> /usr/local/bin/iniciar-tudo && \
    echo 'echo "0 2147483647" > /proc/sys/net/ipv4/ping_group_range' >> /usr/local/bin/iniciar-tudo && \
    echo "cron" >> /usr/local/bin/iniciar-tudo && \
    echo "ntpd" >> /usr/local/bin/iniciar-tudo && \
    echo "ros console-init" >> /usr/local/bin/iniciar-tudo && \
    \
    # Alterando as permissoes do script acima
    chmod 100 /usr/local/bin/iniciar-tudo

ENTRYPOINT ["/usr/bin/ros", "entrypoint"]
