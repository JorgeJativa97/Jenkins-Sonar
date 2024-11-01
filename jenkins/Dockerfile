# Usa la imagen oficial de Jenkins
FROM jenkins/jenkins:lts

# Instala las dependencias necesarias
USER root


RUN apt-get update && \
    apt-get install -y \
    curl \
    apt-transport-https \
    ca-certificates \
    lsb-release \
    sudo \
    php-cli \
    php-mbstring \
    php-xml \
    php-curl \
    php-zip && \
    # Instala Docker
    curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - && \
    echo "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get install -y docker-ce docker-ce-cli containerd.io && \
     # Instala docker-compose
     curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
     chmod +x /usr/local/bin/docker-compose && \
     # Instala Composer
     curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    # Instala PHPUnit
    curl -L https://phar.phpunit.de/phpunit.phar -o /usr/local/bin/phpunit && \
    chmod +x /usr/local/bin/phpunit && \
     # Limpia la caché de apt
    apt-get clean && rm -rf /var/lib/apt/lists/*
    

# Crea el grupo docker y añade el usuario jenkins a ese grupo
RUN groupadd -g 999 docker || true && \
    usermod -aG docker jenkins

# Cambia al usuario jenkins para la instalación de plugins
USER jenkins

# Expon los puertos
EXPOSE 8080
EXPOSE 50000

# Volumen para los datos de Jenkins
VOLUME /var/jenkins_home

# Comando por defecto
CMD ["java", "-jar", "/usr/share/jenkins/jenkins.war"]
