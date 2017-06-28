FROM ubuntu:14.04
MAINTAINER Marcus Eriksson <krummas@gmail.com>

# Make sure the package repository is up to date.

RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install -y git
# Install a basic SSH server
RUN apt-get install -y openssh-server
RUN sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd
RUN mkdir -p /var/run/sshd

# Install JDK 8 (repo added above)
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:webupd8team/java
RUN apt-get update
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
RUN apt-get install -y oracle-java8-installer

# Add user jenkins to the image
RUN adduser --quiet jenkins
# Set password for the jenkins user (you may want to alter this).
RUN echo "jenkins:jenkins" | chpasswd

RUN apt-get install -y ant
RUN su -l jenkins -c "echo -e \"\n\n\n\" | ssh-keygen -t rsa -N \"\""
RUN cat ~jenkins/.ssh/id_rsa.pub
RUN su -l jenkins -c "echo -e \"Host 192.168.1.51\n\tStrictHostKeyChecking no\n\" > ~jenkins/.ssh/config"
# Standard SSH port
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]