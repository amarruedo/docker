FROM alpine:3.4
MAINTAINER asiermarruedo@gmail.com

RUN apk upgrade --no-cache && apk add --no-cache bash curl wget sudo zip unzip vim 

ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

# Dependencies to execute Android builds
RUN dpkg --add-architecture i386
RUN apt-get update -qq
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y openjdk-8-jdk libc6:i386 libstdc++6:i386 libgcc1:i386 libncurses5:i386 libz1:i386

# ------------------------------------------------------
# --- Download Android SDK tools into $ANDROID_HOME

ENV ANDROID_SDK_VERSION 24.4.1
RUN cd /opt && wget -q https://dl.google.com/android/android-sdk_r${ANDROID_SDK_VERSION}-linux.tgz -O android-sdk.tgz \
  && tar -xvzf android-sdk.tgz \
  && rm -f android-sdk.tgz \
  && chown -R root:root ${ANDROID_HOME} \

# ------------------------------------------------------
# --- Install Android SDKs and other build packages

# Other tools and resources of Android SDK
#  you should only install the packages you need!
# To get a full list of available options you can use:
#  android list sdk --no-ui --all --extended
# (!!!) Only install one package at a time, as "echo y" will only work for one license!
#       If you don't do it this way you might get "Unknown response" in the logs,
#         but the android SDK tool **won't** fail, it'll just **NOT** install the package.
#  && (echo y | android update sdk --no-ui --all --filter tools | grep 'package installed') \
 && (echo y | android update sdk --no-ui --all --filter platform-tools | grep 'package installed') \

# SDKs
# Please keep these in descending order!
 && (echo y | android update sdk --no-ui --all --filter android-25 | grep 'package installed') \

# build tools
# Please keep these in descending order!
 && (echo y | android update sdk --no-ui --all --filter build-tools-25.0.2 | grep 'package installed') \
 && (echo y | android update sdk --no-ui --all --filter build-tools-25.0.0 | grep 'package installed') \

# Android System Images, for emulators
# Please keep these in descending order!
 && (echo y | android update sdk --no-ui --all --filter sys-img-x86_64-google_apis-25 | grep 'package installed') \

# Extras
 && (echo y | android update sdk --no-ui --all --filter extra-android-m2repository | grep 'package installed') \
 && (echo y | android update sdk --no-ui --all --filter extra-google-m2repository | grep 'package installed') \
 && (echo y | android update sdk --no-ui --all --filter extra-google-market_apk_expansion | grep 'package installed') \
 && (echo y | android update sdk --no-ui --all --filter extra-google-play_billing | grep 'package installed')

RUN mkdir -p ${ANDROID_HOME}/licenses
COPY licenses/ ${ANDROID_HOME}/licenses
RUN chmod 777 ${ANDROID_HOME}

# ------------------------------------------------------
# --- Cleanup

# Cleaning
RUN apt-get clean

