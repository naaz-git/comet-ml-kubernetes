# Use the Jenkins official image as the base image (ensure amd64 compatibility)
FROM jenkins/jenkins:lts

# Switch to root user to install dependencies (to install Docker and utilities)
USER root

# Enable amd64 architecture (important for compatibility with x86 systems)
RUN dpkg --add-architecture amd64

# Install Docker
RUN apt-get update -y && \
    apt-get install -y ca-certificates curl gnupg && \
    install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    chmod a+r /etc/apt/keyrings/docker.gpg && \
    echo \
      "deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian bullseye stable" | \
      tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update -y && \
    apt-get install -y docker-ce docker-ce-cli containerd.io && \
    apt-get clean

# Add the Jenkins user to the Docker group (to access Docker from inside the container)
RUN groupadd -f docker && \
    usermod -aG docker jenkins

# Set up Docker volume for Docker-in-Docker (DinD)
RUN mkdir -p /var/lib/docker
VOLUME /var/lib/docker

# Switch back to the Jenkins user
USER jenkins

# Expose port for Jenkins
EXPOSE 8080
