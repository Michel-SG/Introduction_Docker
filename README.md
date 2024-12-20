# Introduction_Docker
## 2. Pour Ubuntu 22.04 (Jammy) ou supérieur
## libssl1.1 n'est plus disponible dans les dépôts par défaut. Vous pouvez télécharger et installer manuellement le paquet :


FROM ubuntu:22.04

## Installer les dépendances nécessaires
RUN apt-get update && apt-get install -y wget && rm -rf /var/lib/apt/lists/*

## Télécharger et installer libssl1.1
RUN wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_amd64.deb \
    && dpkg -i libssl1.1_1.1.1f-1ubuntu2_amd64.deb \
    && rm libssl1.1_1.1.1f-1ubuntu2_amd64.deb

## Copier et exécuter votre application
COPY your_application /usr/local/bin/
CMD ["/usr/local/bin/your_application"]



# Solution Alternative : Installer libssl3 et Mettre à Jour Votre Application
## Si vous pouvez modifier l'application ou les dépendances, envisagez de passer à libssl3, qui est inclus par défaut dans Ubuntu 22.04.

FROM ubuntu:22.04

# Installer libssl3 et les dépendances
RUN apt-get update && apt-get install -y libssl3 openjdk-11-jdk git curl && rm -rf /var/lib/apt/lists/*

# Configurer l'environnement pour Jenkins
# (Les commandes pour l'agent Jenkins restent les mêmes que dans la première solution)

# ...


# Exemple Complet de Dockerfile
## Voici un Dockerfile complet vérifiant que libssl1.1 est installé correctement et fonctionne pour une application tierce :

FROM ubuntu:22.04

# Installer wget pour télécharger libssl1.1
RUN apt-get update && apt-get install -y wget && rm -rf /var/lib/apt/lists/*

# Télécharger et installer libssl1.1 depuis les archives Ubuntu
RUN wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2.16_amd64.deb \
    && dpkg -i libssl1.1_1.1.1f-1ubuntu2.16_amd64.deb \
    && rm libssl1.1_1.1.1f-1ubuntu2.16_amd64.deb

# Vérifier la version d'OpenSSL et la bibliothèque utilisée
RUN openssl version && ldd /usr/bin/openssl | grep libssl

# Vérifier que libssl.so.1.1 existe
RUN if [ -f /usr/lib/x86_64-linux-gnu/libssl.so.1.1 ] || [ -f /lib/x86_64-linux-gnu/libssl.so.1.1 ]; then \
        echo "libssl.so.1.1 found"; \
    else \
        echo "Error: libssl.so.1.1 not found!" && exit 1; \
    fi

# Copier et exécuter une application pour validation
COPY your_application /usr/local/bin/
RUN /usr/local/bin/your_application --version

# Nettoyer
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
