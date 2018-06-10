FROM openjdk:jdk

LABEL maintainer="mathieu.brunot at monogramm"

ENV NODE_VERSION %%NODE_VERSION%%
ENV ANDROID_VERSION %%ANDROID_VERSION%%

RUN set -ex; \
	apt-get update -q; \
	# Install the latest Node.js 8.x stable official release
	curl -sL https://deb.nodesource.com/setup_${NODE_VERSION}.x | sudo -E bash -; \
	apt-get install -y --no-install-recommends \
		nodejs \
	; \
	# Install the runtime libraries for the ia32/i386 architecture and G++ compiler
	apt-get install -y --no-install-recommends \
		g++ \
		lib32z1 \
		lib32ncurses5 \
		lib32bz2-1.0 \
		libstdc++6:i386 \
		unzip \
	; \
	rm -rf /var/lib/apt/lists/*; \
	# Set the JAVA_HOME system environment variable
	export JAVA_HOME=$(update-alternatives --query javac | sed -n -e 's/Best: *\(.*\)\/bin\/javac/\1/p'); \
	; \
	# download and extract android sdk
	curl https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip | unzip /usr/local/; \
	export ANDROID_HOME='/usr/local/android-sdk-linux'; \
	$ANDROID_HOME/tools/bin/sdkmanager \
		"tools" \
		"platform-tools" \
		"platforms;android-25" \
		"build-tools;27.0.3" \
		"extras;android;m2repository" \
		"extras;google;m2repository"\
	; \
	# Install the NativeScript CLI
	npm install nativescript @angular/cli -g --unsafe-perm

#ENV JAVA_HOME /usr/local/android-sdk-linux
#ENV ANDROID_HOME /usr/local/android-sdk-linux
ENV PATH $PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools

ENV GRADLE_USER_HOME /src/gradle
VOLUME /src
WORKDIR /src

# Verify install
RUN set -ex; \
	ng --version; \
	tns doctor