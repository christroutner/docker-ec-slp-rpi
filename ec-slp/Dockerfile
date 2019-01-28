#IMAGE BUILD COMMANDS
FROM python-base
MAINTAINER Chris Troutner <chris.troutner@gmail.com>

# Get the SLP fork for Electron cash
RUN wget https://github.com/simpleledger/Electron-Cash-SLP/archive/3.4.3.tar.gz
RUN tar -xvf 3.4.3.tar.gz
WORKDIR /home/safeuser/Electron-Cash-SLP-3.4.3

# Log in as the application user
USER safeuser
# supply password for sudo commands
RUN echo 'abcd8765' | sudo -S pwd
WORKDIR /home/safeuser

# Install other required libraries
RUN sudo apt install -y python3-pyqt5 python3-socks
RUN sudo pip3 install dnspython
RUN sudo pip3 install google-api-python-client
RUN sudo pip3 install protobuf
RUN sudo pip3 install jsonrpc
RUN sudo pip3 install jsonrpclib
RUN sudo pip3 install jsonrpclib-pelix

# Copy config files.
RUN mkdir /home/safeuser/.electron-cash
COPY config /home/safeuser/.electron-cash/config
RUN sudo chown safeuser /home/safeuser/.electron-cash/config
RUN mkdir /home/safeuser/.electron-cash/wallets
COPY default_wallet /home/safeuser/.electron-cash/wallets/default_wallet

# Expose the port defined in the config file.
EXPOSE 5111

COPY dummyapp.js dummyapp.js
CMD ["node", "dummyapp.js"]

#COPY launch-electron.sh launch-electron.sh
#CMD ["./launch-electron.sh"]
