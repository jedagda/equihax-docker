FROM kalilinux/kali
# Metadata params
ARG BUILD_DATE
ARG VERSION
ARG VCS_URL
ARG VCS_REF

LABEL org.label-schema.build-date=$BUILD_DATE \
      org.label-schema.vcs-url=$VCS_URL \
      org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.version=$VERSION \
      org.label-schema.name='Kali Linux' \
      org.label-schema.description='Official Kali Linux docker image' \
      org.label-schema.usage='https://www.kali.org/news/official-kali-linux-docker-images/' \
      org.label-schema.url='https://www.kali.org/' \
      org.label-schema.vendor='Offensive Security' \
      org.label-schema.schema-version='1.0' \
      org.label-schema.docker.cmd='docker run --rm kalilinux/kali-linux-docker' \
      org.label-schema.docker.cmd.devel='docker run --rm -ti kalilinux/kali-linux-docker' \
      org.label-schema.docker.debug='docker logs $CONTAINER' \
      io.github.offensive-security.docker.dockerfile="Dockerfile" \
      io.github.offensive-security.license="GPLv3" \
      MAINTAINER="Joshua Dagda jedagda@miners.utep.edu"

ENV DEBIAN_FRONTEND noninteractive

# Copy the motd intro file to samsepi0l container
COPY ./text-files/motd /etc/motd


RUN apt-get update  


RUN apt-get install -y apt-get software-properties-common sudo lvm2 openssh-server vim notmuch notmuch-vim net-tools nmap curl 
RUN curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && \
  chmod 755 msfinstall && \
  ./msfinstall 

RUN echo "Set disable_coredump false" >> /etc/sudoers.conf

RUN mkdir /var/run/sshd
RUN groupadd dedsec
RUN useradd -ms /bin/bash -g dedsec samsepi0l
RUN usermod -a -G sudo samsepi0l
RUN usermod -g dedsec samsepi0l
#RUN echo "%dedsec         ALL : (ALL) ALL" >> /etc/sudoers
RUN echo 'samsepi0l:dedsecrox!' | chpasswd


RUN sudo -u samsepi0l vim-addon-manager install notmuch

# Add and install lolcat for a colorful RGB experience
COPY lolcat-master /samsepi0l/
WORKDIR /samsepi0l/lolcat-master/bin/ 
RUN gem install lolcat


#Create unimatrix directory and copy unimatrix script for cool matrix introduction
RUN mkdir /usr/local/bin/unimatrix
COPY unimatrix.py /usr/local/bin/unimatrix
RUN chmod a+rx /usr/local/bin/unimatrix

#We support 256color, right? If not, you can always change this environment variable.
ENV TERM=xterm-256color

#Running ssh with keys is a good thing, right?
RUN sed -i 's/RSAAuthentication yes/RSAAuthentication no/' /etc/ssh/ssh_config

#Start the ssh service
CMD service ssh start && nginx -g 'daemon off;'
CMD ["/usr/sbin/sshd", "-D"]


#RUN echo '[ ! -z "$TERM" -a -r /etc/motd ] && cat /etc/issue && cat /etc/motd'  >> /etc/bash.bashrc

# These files are only pertaining to the slowloris workshop

WORKDIR /home/samsepi0l
RUN mkdir slowloris
COPY ./slowloris/ /home/samsepi0l/slowloris/
COPY ./text-files/README /home/samsepi0l/slowloris/
RUN chown samsepi0l:dedsec -R /home/samsepi0l/slowloris/
RUN mkdir mail/
COPY ./text-files/hellofriend.eml /home/samsepi0l/mail/
RUN chown samsepi0l:dedsec -R /home/samsepi0l/mail/



#ENV NOTVISIBLE "in users profile"
#RUN echo "export VISIBLE=now" >> /etc/profile

#RUN apt-get update \
#    && apt-get dist-upgrade \
#    && apt-get clean
#ENTRYPOINT bash
#EXPOSE 22

