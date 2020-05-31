FROM jenkins/jenkins:lts-alpine

USER root
RUN apk add --no-cache python3 make docker && \
    pip3 install pytest

USER jenkins
# Disable the setup Wizard
ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"
COPY set_admin_pw.groovy /usr/share/jenkins/ref/init.groovy.d/set_admin_pw.groovy
# Install only the needed plugins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt
