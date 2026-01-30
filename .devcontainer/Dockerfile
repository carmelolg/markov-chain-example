FROM python:3.11.13-bullseye

# Set the workspace enviroment variable Worspace
ENV WORKSPACE_BASE_PATH=/workspaces/dev-container-markov

# Copy the private key into the image
COPY ../private_key /workspaces/dev-container-markov/private_key
RUN chmod 600 /workspaces/dev-container-markov/private_key
RUN echo "eval \$(ssh-agent -s)" >> /etc/bash.bashrc && \
    echo "ssh-add /workspaces/dev-container-markov/private_key" >> /etc/bash.bashrc


# Install sshpass
#RUN apt-get update -y && \
#    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends sshpass

RUN echo 'SSHD: ALL' >> /etc/hosts.allow

# Upgrade pip and install necessary Python packages
RUN python3 -m pip install --upgrade pip
RUN python3 -m pip install ansible ansible-lint ansible-dev-tools ansible-builder ansible-creator ansible-navigator pywinrm molecule pyVmomi requests \
    docker netmiko yamllint dnspython pytest-molecule

# Docker specs to use with molecule
RUN apt-get update && \
    apt-get install -y apt-transport-https ca-certificates curl gnupg-agent less software-properties-common && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" && \
    apt-get update && \
    apt-get install -y docker-ce-cli && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*


COPY requirements.txt .
RUN pip install --upgrade pip
RUN pip install -r requirements.txt

# Change Back to correct workdir
WORKDIR ${WORKSPACE_BASE_PATH}
