#IMAGE BUILD COMMANDS
FROM python-base
MAINTAINER Chris Troutner <chris.troutner@gmail.com>

# Log in as the application user
USER safeuser
# supply password for sudo commands
RUN echo 'abcd8765' | sudo -S pwd
WORKDIR /home/safeuser

# Used to give Docker something to chew on, so it doesn't exit.
COPY dummyapp.js dummyapp.js

# Get the SLP fork for Electron cash
RUN wget https://github.com/simpleledger/Electron-Cash-SLP/archive/3.4.3.tar.gz
RUN tar -xvf 3.4.3.tar.gz
WORKDIR /home/safeuser/Electron-Cash-SLP-3.4.3

# Install python dependencies.
# Expect it to fail.
RUN sudo python3 setup.py install || echo "done"

# Install other required libraries
RUN sudo apt install -y python3-pyqt5 python3-socks
RUN sudo pip3 install dnspython
RUN sudo pip3 install google-api-python-client
RUN sudo pip3 install protobuf
RUN sudo pip3 install jsonrpc
RUN sudo pip3 install jsonrpclib
RUN sudo pip3 install jsonrpclib-pelix
RUN sudo pip3 install pyaes
RUN sudo pip3 install ecdsa
RUN sudo pip3 install urllib3
RUN sudo pip3 install chardet
RUN sudo pip3 install certifi

# Copy config files.
RUN mkdir /home/safeuser/.electron-cash
COPY config /home/safeuser/.electron-cash/config
RUN sudo chown safeuser /home/safeuser/.electron-cash/config
RUN mkdir /home/safeuser/.electron-cash/wallets
COPY default_wallet /home/safeuser/.electron-cash/wallets/default_wallet

# Expose the port defined in the config file.
EXPOSE 5111

COPY launch-electron.sh launch-electron.sh
CMD ["./launch-electron.sh"]

#CMD ["node", "dummyapp.js"]
