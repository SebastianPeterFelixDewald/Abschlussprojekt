FROM jenkins/jenkins:2.260-jdk11
USER root

RUN apt-get update && apt-get install -y && \
  
  # Installation von wget
  apt-get install -y wget && \
  
  # Installation von Maven 3.6.3 via wget und Platzierung im richtigen Unterordner
  wget https://mirror.softaculous.com/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip && \
  unzip apache-maven-3.6.3-bin.zip && \
  mv apache-maven-3.6.3 /usr/share/maven && \
  rm apache-maven-3.6.3-bin.zip && \
  
  # sshpass ist notwendig für ssh Befehle in Ansible
  apt-get install sshpass && \
  
  # Installation von Python 3.7 und dem Pip Modul
  apt-get install python3-pip -y && \
  
  # Ansible Installation in der aktuellsten Version
  pip3 install ansible --upgrade && \
  
  # Docker SDK inkl. Docker CLI für Docker commands innerhalb von Jenkins
  pip3 install docker && \
  
  # Docker-compose
  pip3 install docker-compose && \
  
  # lxml ist relevant für die Ansible Galaxy Collection, um z.B. ein .war file von Nexus zu ziehen und zu deployen
  pip3 install lxml && \
  
  # Angesprochene Ansible Plugins, die Ansible um zahlreiche Befehle z.B. für Maven erweitern
  ansible-galaxy collection install community.general && \
  
  # Optionales Modul für Azure & Ansible, Funktion noch nicht getestet
  ansible-galaxy install Azure.azure_preview_modules && \
  pip3 install -r ~/.ansible/roles/Azure.azure_preview_modules/files/requirements-azure.txt && \
  
  # Letztes Update aller Pakete
  apt-get upgrade -y && \
  apt-get update

# Ansible Configuration Datei wird hinzugefügt - Mit dieser können verschiedene Warnings beispielsweise ausgeschaltet werden
COPY ansible/ansible.cfg etc/ansible/ansible.cfg

# Definition des Maven Pfads und der maximalen Ressourcen von Jenkins & Java
ENV MAVEN_HOME /usr/share/maven
ENV M2_HOME /usr/share/maven
ENV PATH $M2_HOME/bin/:$PATH
ENV JAVA_OPTS="-Xmx8192m"
ENV JENKINS_OPTS=" --handlerCountMax=300"

