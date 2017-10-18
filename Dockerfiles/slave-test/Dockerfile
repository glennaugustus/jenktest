FROM ubuntu
MAINTAINER Glenn Augustus <gaugustu@dxc.com>

# Install dev requirements
# NOTE : Keep the following list in alphanumerical order.
RUN apt-get update && apt-get install -y \
   openssh-server \
   openjdk-8-jdk
RUN sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd

RUN mkdir /var/run/sshd
RUN adduser --disabled-password --gecos "" --quiet jenkins
RUN echo "jenkins:jenkins" | chpasswd
EXPOSE 22

USER jenkins
#Make ssh dir
RUN mkdir /home/jenkins/.ssh

# DO NOT EVER DO THE FOLLOWING THINGS (SECURITY) :
#COPY jenkins_rsa /home/jenkins/.ssh/id_rsa
#COPY jenkins_rsa.pub /home/jenkins/.ssh/id_rsa.pub
#USER root
#RUN chown jenkins:jenkins /home/jenkins/.ssh/id_rsa
#RUN chown jenkins:jenkins /home/jenkins/.ssh/id_rsa.pub
#USER jenkins
#RUN chmod 600 /home/jenkins/.ssh/id_rsa
#RUN chmod 600 /home/jenkins/.ssh/id_rsa.pub
#USER jenkins
#RUN /bin/bash -c "eval '$(ssh-agent -s)'; ssh-add /home/jenkins/.ssh/id_rsa"

# Create known_hosts
RUN touch /home/jenkins/.ssh/known_hosts
# Add github (or your git server) fingerprint to known hosts
RUN ssh-keyscan -t rsa github.com >> /home/jenkins/.ssh/known_hosts
RUN ssh-keyscan -t rsa github.dxc.com >> /home/jenkins/.ssh/known_hosts

USER root
CMD ["/usr/sbin/sshd", "-D"]

