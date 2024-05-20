# Start with the official Ubuntu base image
FROM ubuntu:latest

# Install necessary packages
RUN apt-get update && \
    apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release \
    git \
    openjdk-11-jdk && \ # Jenkins agents generally require Java
    rm -rf /var/lib/apt/lists/*

# Install Docker
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list && \
    apt-get update && \
    apt-get install -y docker-ce docker-ce-cli containerd.io && \
    rm -rf /var/lib/apt/lists/*

# Create Jenkins agent directory
RUN mkdir -p /home/jenkins/agent && \
    useradd -m -d /home/jenkins jenkins && \
    chown -R jenkins:jenkins /home/jenkins

# Switch to jenkins user
USER jenkins

# Set working directory
WORKDIR /home/jenkins/agent

# Default command: run the Jenkins agent
CMD java -jar /usr/share/jenkins/agent.jar -jnlpUrl http://172.17.0.2:8080/computer/agent1/jenkins-agent.jnlp -secret 0692d652e265bb486f6c1912403d3361dfb3d29d1247a7516fabc138046b981d -workDir /home/jenkins/agent
