ARG VARIANT="focal"
FROM mcr.microsoft.com/vscode/devcontainers/base:${VARIANT}

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

ENV JAVA_VERSION="8"
# comes from https://developer.android.com/studio/#command-tools
ENV ANDROID_SDK_TOOLS_VERSION="8092744"
ENV ANDROID_TOOLS_URL="https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_SDK_TOOLS_VERSION}_latest.zip"
ENV ANDROID_VERSION="29"
ENV ANDROID_BUILD_TOOLS_VERSION="29.0.3"
ENV ANDROID_ARCHITECTURE="x86_64"
ENV ANDROID_SDK_ROOT="/home/$USERNAME/android"
ENV PATH="$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/cmdline-tools/latest:$ANDROID_SDK_ROOT/emulator:$ANDROID_SDK_ROOT/platform-tools:$ANDROID_SDK_ROOT/platforms:$PATH"

ENV DEBIAN_FRONTEND="noninteractive"
RUN apt-get update && apt-get install --yes --no-install-recommends openjdk-$JAVA_VERSION-jdk 

USER $USERNAME

# android sdk
RUN mkdir -p $ANDROID_SDK_ROOT \
  && cd $ANDROID_SDK_ROOT \
  && mkdir -p /home/$USERNAME/.android \
  && curl -o android_tools.zip $ANDROID_TOOLS_URL \
  && mkdir -p $ANDROID_SDK_ROOT/cmdline-tools \
  && unzip -qq -d "$ANDROID_SDK_ROOT/cmdline-tools" android_tools.zip \
  && rm android_tools.zip \
  && mv $ANDROID_SDK_ROOT/cmdline-tools/cmdline-tools $ANDROID_SDK_ROOT/cmdline-tools/latest 

RUN touch /home/$USERNAME/.android/repositories.cfg \
  && yes "y" | sdkmanager "build-tools;$ANDROID_BUILD_TOOLS_VERSION" \
  && yes "y" | sdkmanager "platforms;android-$ANDROID_VERSION" \
  && yes "y" | sdkmanager "platform-tools" 
