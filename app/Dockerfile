FROM jenkins/jenkins
USER root

# Update the image.
# Get requried packacges to support later download of other files.
# Install bash-completion to make it easier when accessing the console of a running container.
# Install AWSCLI
# Add Hashicorp to sources list.
# Update the APT database and perform any upgrades of existing installed packages.
# Install Terraform.
# Install NodeJS and NPM.
# Install sonar scanner.
# Clean up APT installation cache.


#------ Installation Woerkflow ---#
RUN apt update && apt-get install -y lsb-release && \
apt install -y unzip wget curl bash-completion && \
apt install -y awscli && \
apt install -y sudo && \
wget -O- https://apt.releases.hashicorp.com/gpg \
| sudo gpg --dearmor \
| sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null && \
sudo gpg --no-default-keyring --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg --fingerprint && \
sudo echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
     https://apt.releases.hashicorp.com $(lsb_release -cs) main" \
     | sudo tee /etc/apt/sources.list.d/hashicorp.list && \
sudo apt update && sudo apt -y upgrade && \
sudo apt install -y terraform && \
sudo apt install -y nodejs npm && \
sudo curl -fsSL https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-7.0.2.4839-linux-aarch64.zip -o /tmp/sonar-scanner.zip && \
sudo unzip /tmp/sonar-scanner.zip -d /opt/ && \
sudo mv /opt/sonar-scanner-* /opt/sonar-scanner && \
sudo ln -s /opt/sonar-scanner/bin/sonar-scanner /usr/local/bin/sonar-scanner && \
sudo rm /tmp/sonar-scanner.zip  

# Switch to user jenkins.
USER jenkins

# Copy file that lists plugins to temporary directory.
COPY --chown=jenkins:jenkins ./plugins.txt /tmp/

# Install plugins from plugins.txt file.
# delete plugin file from temporary directory.

RUN jenkins-plugin-cli --plugins -f /tmp/plugins.txt && \
rm /tmp/plugins.txt
