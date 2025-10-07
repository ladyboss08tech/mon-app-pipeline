FROM python:3.9-slim 
# Étape 1: Installer les dépendances Python avec pip 
# On fige les versions pour garantir la compatibilité 
RUN pip install ansible requests==2.31.0 docker==5.0.3 
# Étape 2: Installer la collection Ansible pour Docker 
RUN ansible-galaxy collection install community.docker 
WORKDIR /ansible 