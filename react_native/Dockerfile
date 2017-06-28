## -*- docker-image-name: "react-native" -*-

# Docker image for react native experimentation

FROM node:latest

MAINTAINER Giles Paterson <giles@vurt.uk>

LABEL version="1.0.2"

# Setup environment variables
ENV PATH $PATH:node_modules/.bin

##
## Install Java
##
RUN DEBIAN_FRONTEND=noninteractive echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | tee /etc/apt/sources.list.d/webupd8team-java.list && \
    echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list && \
    apt-get update -q && apt-get -qy --no-install-recommends install software-properties-common && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886 && \
    apt-get update && \
    echo "oracle-java8-installer shared/accepted-oracle-license-v1-1 select true" | debconf-set-selections && \
    apt-get -qy --no-install-recommends install oracle-java8-installer oracle-java8-unlimited-jce-policy oracle-java8-set-default 

#RUN DEBIAN_FRONTEND=noninteractive apt-get update -q && \
#	apt-get install -qy --no-install-recommends sudo default-jdk

##
## Install Android SDK
##
# Set correct environment variables.
#ENV ANDROID_SDK_FILE android-sdk_r23.0.1-linux.tgz
#ENV ANDROID_SDK_URL http://dl.google.com/android/$ANDROID_SDK_FILE

# Install 32bit support for Android SDK
RUN dpkg --add-architecture i386 && \
    apt-get update -q && \
    apt-get install -qy --no-install-recommends libstdc++6:i386 libgcc1:i386 zlib1g:i386 libncurses5:i386

# Install kvm support for Android emulator
#RUN apt-get install -qy --no-install-recommends qemu-kvm libvirt-bin libegl1-mesa

# Install Android SDK
ENV ANDROID_HOME /usr/local/android-sdk-linux
RUN cd /usr/local && wget -q https://dl.google.com/android/android-sdk_r24.4.1-linux.tgz -O android-sdk.tgz
RUN cd /usr/local && tar -xvzf android-sdk.tgz
RUN cd /opt && rm -f android-sdk.tgz

RUN export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools && \
    chgrp -R users $ANDROID_HOME && \
    chmod -R 0775 $ANDROID_HOME

#RUN cd /usr/local && \
#    wget $ANDROID_SDK_URL && \
#    tar -xzf $ANDROID_SDK_FILE && \
#    export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools && \
#    chgrp -R users $ANDROID_HOME && \
#    chmod -R 0775 $ANDROID_HOME && \
#    rm $ANDROID_SDK_FILE

# Install android tools and system-image.

ENV PATH $PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$ANDROID_HOME/build-tools/23.0.1
#RUN echo "y" | android update sdk \
#    --no-ui \
#    --force \
#    --all \
#    --filter platform-tools,android-23,build-tools-23.0.1,extra-android-support,extra-android-m2repository,sys-img-x86_64-android-23,extra-google-m2repository

#RUN echo "y" | android update sdk --no-ui --force --all --filter "platform-tools" \
# && echo "y" | android update sdk --no-ui --force --all --filter "android-23" \
# && echo "y" | android update sdk --no-ui --force --all --filter "build-tools-23.0.1" \
# && echo "y" | android update sdk --no-ui --force --all --filter "extra-android-support" \
# && echo "y" | android update sdk --no-ui --force --all --filter "extra-android-m2repository" \
# && echo "y" | android update sdk --no-ui --force --all --filter "sys-img-x86_64-android-23" \
# && echo "y" | android update sdk --no-ui --force --all --filter "extra-google-m2repository"

RUN echo y | android update sdk --no-ui --all --filter platform-tools | grep 'package installed'
RUN echo y | android update sdk --no-ui --all --filter extra-android-support

# SDKs
# Please keep these in descending order!
RUN echo y | android update sdk --no-ui --all --filter android-25
RUN echo y | android update sdk --no-ui --all --filter android-24
RUN echo y | android update sdk --no-ui --all --filter android-23
RUN echo y | android update sdk --no-ui --all --filter android-18
RUN echo y | android update sdk --no-ui --all --filter android-16

# build tools
# Please keep these in descending order!
RUN echo y | android update sdk --no-ui --all --filter build-tools-25.0.3
RUN echo y | android update sdk --no-ui --all --filter build-tools-25.0.2
RUN echo y | android update sdk --no-ui --all --filter build-tools-25.0.1
RUN echo y | android update sdk --no-ui --all --filter build-tools-25.0.0
RUN echo y | android update sdk --no-ui --all --filter build-tools-24.0.3
RUN echo y | android update sdk --no-ui --all --filter build-tools-24.0.2
RUN echo y | android update sdk --no-ui --all --filter build-tools-24.0.1
RUN echo y | android update sdk --no-ui --all --filter build-tools-23.0.3
RUN echo y | android update sdk --no-ui --all --filter build-tools-23.0.2
RUN echo y | android update sdk --no-ui --all --filter build-tools-23.0.1

# Android System Images, for emulators
# Please keep these in descending order!
#RUN echo y | android update sdk --no-ui --all --filter sys-img-x86_64-android-25
#RUN echo y | android update sdk --no-ui --all --filter sys-img-x86-android-25
#RUN echo y | android update sdk --no-ui --all --filter sys-img-armeabi-v7a-android-25

RUN echo y | android update sdk --no-ui --all --filter sys-img-x86_64-android-24
RUN echo y | android update sdk --no-ui --all --filter sys-img-x86-android-24
RUN echo y | android update sdk --no-ui --all --filter sys-img-armeabi-v7a-android-24

#RUN echo y | android update sdk --no-ui --all --filter sys-img-x86-android-23
#RUN echo y | android update sdk --no-ui --all --filter sys-img-armeabi-v7a-android-23

# Extras
RUN echo y | android update sdk --no-ui --all --filter extra-android-m2repository
RUN echo y | android update sdk --no-ui --all --filter extra-google-m2repository

#RUN $ANDROID_HOME/tools/android-accept-licenses.sh android update sdk --no-ui --obsolete --force


##
## Install react native
##
RUN npm install -g react-native-cli

##
## Install Ignite cli
##
RUN npm install -g ignite-cli

# Clean up when done.
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    npm cache clean --force

# Default react-native web server port
EXPOSE 8081

ENV USERNAME dev

RUN adduser --disabled-password --gecos '' $USERNAME && \
    echo $USERNAME:$USERNAME | chpasswd && \
    echo "%sudo ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \
    adduser $USERNAME sudo
    #adduser $USERNAME kvm && \
    #adduser $USERNAME libvirt

# Create android avd image
# RUN echo "no" | android create avd -n android-23-phone -c 1000M -s WVGA854 -t 1

# Add Tini
ENV TINI_VERSION v0.6.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

USER $USERNAME

# Set workdir
# You'll need to run this image with a volume mapped to /home/dev (i.e. -v $(pwd):/home/dev) or override this value
WORKDIR /home/$USERNAME/app
#RUN sudo chown -R $USERNAME:$USERNAME /home/$USERNAME/app

# Tell gradle to store dependencies in a sub directory of the android project
# this persists the dependencies between builds
ENV GRADLE_USER_HOME /home/$USERNAME/app/android/gradle_deps

ENTRYPOINT ["/tini", "--"]

# Run your program under Tini
# CMD ["/your/program", "-and", "-its", "arguments"]
# or docker run your-image /your/program ...
