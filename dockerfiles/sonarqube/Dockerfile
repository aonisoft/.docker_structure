FROM sonarqube:lts

USER root

COPY entrypoint.sh $SONARQUBE_HOME/docker

RUN mkhomedir_helper sonarqube

RUN apt-get update && apt-get -y install unzip 

RUN wget "https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip"

RUN unzip sonar-scanner-cli-5.0.1.3006-linux.zip

RUN rm sonar-scanner-cli-5.0.1.3006-linux.zip

RUN mv sonar-scanner-5.0.1.3006-linux $SONARQUBE_HOME/sonar_scanner

ENV PATH=$SONARQUBE_HOME/sonar_scanner/bin:$PATH

USER sonarqube

WORKDIR /website_test