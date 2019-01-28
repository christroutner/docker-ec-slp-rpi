# Builds a Docker container python libraries needed to run Electron Cash

FROM christroutner/ct-base-ubuntu
MAINTAINER Chris Troutner <chris.troutner@gmail.com>

# Update base OS.
RUN apt-get update

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
RUN sudo python3 setup.py install
