FROM mcr.microsoft.com/dotnet/core/sdk:2.2.105
 
# Set environment variables
# Need directories to be accessible by "other rwx" as Jenkins runs everything as a arbitary user id - 0777
ENV HOME /var/lib/agent
ENV TEMP_DIR /tmp
ENV ANGULAR_VERSION 7.3.5
ENV SONAR_USER_HOME /var/lib/agent/.sonar

# Added dotnet tool and NodeJS binary install location to the path.
ENV PATH $PATH:/var/lib/agent/.dotnet/tools:/var/lib/agent/node_modules/.bin:/var/lib/agent/sonar-scanner/bin
 
# Install NodeJS, main Angular package, dotnet sonarscanner and accompanying packages
RUN adduser agent --home $HOME && \
    usermod -aG staff agent && \
    mkdir $HOME/.ssh && \
    apt-get update && \
    apt-get -y install libsystemd0=232-25+deb9u11 libexpat1=2.2.0-2+deb9u2 libudev1=232-25+deb9u11 libssh2-1=1.7.0-1+deb9u1 && \
    apt-get -y --no-install-recommends install curl gnupg chromium && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    apt-get -y --no-install-recommends install nodejs openjdk-8-jre build-essential libperl5.24 libssl1.1 openssl perl perl-base perl-modules-5.24 git zip unzip && \
    npm install -g npm && npm install && \
    npm install -g dockerlint && \
    curl --insecure -o ./sonarscanner.zip -L https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-3.3.0.1492-linux.zip && \
    unzip sonarscanner.zip && \
    rm sonarscanner.zip && \
    mv sonar-scanner-3.3.0.1492-linux /var/lib/jenkins/sonar-scanner && \
    apt-get clean && \
    apt-get -y purge curl wget && \
    apt -y autoremove && \
    dotnet tool install --global dotnet-sonarscanner && \
    apt-get -y --allow-remove-essential remove apt && \
    rm -rf /var/lib/apt/lists/* && \
    npm install -g @angular/cli@${ANGULAR_VERSION} @angular/compiler@7.2.14 @angular/compiler-cli@7.2.14 tslib@1.9.3 typescript@3.2.2 ng-packagr && \
    chmod -R 0777 $TEMP_DIR && \
    chmod -R 0777 $HOME && \
 
WORKDIR /var/lib/agent

USER agent

HEALTHCHECK CMD ng --version
