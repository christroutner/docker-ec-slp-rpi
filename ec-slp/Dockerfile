#IMAGE BUILD COMMANDS
FROM ubuntu:18.04
MAINTAINER Chris Troutner <chris.troutner@gmail.com>

#Update the OS and install any OS packages needed.
RUN apt-get update
RUN apt-get install -y sudo git curl nano gnupg wget

#Install Node and NPM
RUN curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh
RUN bash nodesource_setup.sh
RUN apt-get install -y nodejs build-essential

#Create the user 'safeuser' and add them to the sudo group.
RUN useradd -ms /bin/bash safeuser
RUN adduser safeuser sudo

#Set password to 'abcd8765' change value below if you want a different password
RUN echo safeuser:abcd8765 | chpasswd

#Set the working directory to be the users home directory
WORKDIR /home/safeuser

#Setup NPM for non-root global install (like on a mac)
RUN mkdir /home/safeuser/.npm-global
RUN chown -R safeuser .npm-global
RUN echo "export PATH=~/.npm-global/bin:$PATH" >> /home/safeuser/.profile
RUN runuser -l safeuser -c "npm config set prefix '~/.npm-global'"

COPY dummyapp.js dummyapp.js
#CMD ["node", "dummyapp.js"]

# Log in as the application user
USER safeuser
# supply password for sudo commands
RUN echo 'abcd8765' | sudo -S pwd
WORKDIR /home/safeuser

# Install python tools
RUN sudo apt-get install -y python3-pip python3-setuptools

# Get the SLP fork for Electron cash
RUN wget https://github.com/simpleledger/Electron-Cash-SLP/archive/3.4.3.tar.gz
RUN tar -xvf 3.4.3.tar.gz
WORKDIR /home/safeuser/Electron-Cash-SLP-3.4.3

# Install python dependencies.
#RUN sudo pip3 install -I pyqt5-sip
RUN apt install python3-pyqt5
RUN sudo python3 setup.py install


# Copy config files.
RUN mkdir /home/safeuser/.electron-cash
COPY config /home/safeuser/.electron-cash/config
RUN sudo chown safeuser /home/safeuser/.electron-cash/config
RUN mkdir /home/safeuser/.electron-cash/wallets
COPY default_wallet /home/safeuser/.electron-cash/wallets/default_wallet

# Expose the port defined in the config file.
EXPOSE 5111

#COPY dummyapp.js dummyapp.js
#CMD ["node", "dummyapp.js"]

COPY launch-electron.sh launch-electron.sh
CMD ["./launch-electron.sh"]
