FROM ubuntu:latest
LABEL maintainer="Guisalmeida"

# Atualiza SO
RUN apt-get update && apt-get install -y openssh-server vim curl sudo
RUN apt-get update && apt-get install -y git
RUN mkdir /var/run/sshd

# Configura ssh
RUN echo 'root:root' |chpasswd
RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN mkdir /root/.ssh
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Cria mensagem de entrada no servidor
RUN echo 'Banner /etc/banner' >> /etc/ssh/sshd_config
COPY etc/banner /etc

# Adiciona usuario app
RUN useradd -ms /bin/bash app
RUN adduser app sudo
RUN echo 'app:app' |chpasswd

# Altera para usuário app
USER app

# Instala nvm e node 12
RUN /bin/bash -l -c 'curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash'
RUN /bin/bash -l -c '. ~/.nvm/nvm.sh && nvm install 12'

# Altera para usuário root
USER root

# Expõe as portas
EXPOSE 22
EXPOSE 3090

# Cria e configura o ponto de montagem do volume
RUN mkdir /workspace
RUN chmod 777 /workspace
VOLUME /workspace

CMD    ["/usr/sbin/sshd", "-D"]