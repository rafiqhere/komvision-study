FROM jenkins/jenkins:lts-jdk21

USER root

RUN apt-get update && \
    apt-get install -y \
      ca-certificates \
      curl \
      gnupg \
      lsb-release \
      gosu \
      iproute2 \
      wireguard-tools && \
    install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | \
      gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    chmod a+r /etc/apt/keyrings/docker.gpg && \
    echo \
      "deb [arch=$(dpkg --print-architecture) \
      signed-by=/etc/apt/keyrings/docker.gpg] \
      https://download.docker.com/linux/debian \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get install -y docker-ce-cli && \
    curl -fsSL https://pkgs.netbird.io/install.sh | sh && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

COPY docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh

RUN chmod +x /usr/local/bin/docker-entrypoint.sh

RUN jenkins-plugin-cli --plugins \
    git \
    job-dsl \
    workflow-aggregator \
    workflow-multibranch \
    docker-workflow \
    docker-plugin \
    blueocean \
    oic-auth \
    configuration-as-code \
    role-strategy \
    bitbucket \
    cloudbees-bitbucket-branch-source \
    credentials \
    credentials-binding \
    plain-credentials \
    cloudbees-folder \
    custom-tools-plugin \
    extra-tool-installers \
    msbuild \
    ws-cleanup \
    aws-secrets-manager-secret-source \
    aws-secrets-manager-credentials-provider \
    nodejs \
    terraform

ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]