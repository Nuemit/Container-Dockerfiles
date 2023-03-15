FROM kasmweb/core-ubuntu-focal:1.12.0
USER root
ENV HOME /home/kasm-default-profile
ENV STARTUPDIR /dockerstartup
ENV INST_SCRIPTS $STARTUPDIR/install
WORKDIR $HOME

######### Customize Container Here ###########
## UPDATES
RUN apt-get update
RUN apt-get upgrade -y

# ENABLE sudo
RUN apt-get install -y sudo \
    && echo 'kasm-user ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers \
    && rm -rf /var/lib/apt/list/*

# Install Firefox
RUN wget -O firefox-latest.tar.bz2 "https://download.mozilla.org/?product=firefox-latest&os=linux64&lang=en-US"
RUN tar xjf firefox-*.tar.bz2
RUN mv firefox /opt
RUN ln -s /opt/firefox/firefox /usr/local/bin/firefox
RUN wget https://raw.githubusercontent.com/mozilla/sumo-kb/main/install-firefox-linux/firefox.desktop -P /usr/local/share/applications
RUN cp /usr/local/share/applications/firefox.desktop "$HOME/Desktop"
RUN chmod +x "$HOME/Desktop/firefox.desktop"
RUN rm -f firefox-latest.tar.bz2


# Install Burp Suite
## Install openjdk-17-jre
RUN apt-get install openjdk-17-jre -y

## Install Burp
RUN curl https://portswigger-cdn.net/burp/releases/download\?product\=community\&type\=Linux --output $HOME/01_install_burp.sh 
RUN chmod +x $HOME/01_install_burp.sh
RUN mkdir $HOME/installation_dir_burp
RUN bash $HOME/01_install_burp.sh -q 
RUN rm -rf $HOME/01_install_burp.sh
ARG burp_home="/opt/BurpSuiteCommunity/Burp Suite Community Edition.desktop"
ARG burp_new="$HOME/Desktop"
RUN cp "$burp_home" $burp_new

######### Installation of passwort lists
RUN wget -c https://github.com/danielmiessler/SecLists/archive/master.zip -O SecList.zip \
  && unzip SecList.zip \
  && rm -f SecList.zip


######### End Customizations ###########
RUN chown 1000:0 $HOME
RUN $STARTUPDIR/set_user_permission.sh $HOME
ENV HOME /home/kasm-user
WORKDIR $HOME
RUN mkdir -p $HOME && chown -R 1000:0 $HOME
USER 1000
