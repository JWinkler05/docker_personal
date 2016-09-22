FROM jenkins
COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt

# Set up Jenkins User
ADD config/config.xml /var/jenkins_home/config.xml
ADD config/admin/config.xml /var/jenkins_home/users/admin/config.xml

EXPOSE 22 80 443
