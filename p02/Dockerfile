FROM tomcat
RUN apt-get update && apt-get -y upgrade
WORKDIR /usr/local/tomcat
COPY helloworld.war /usr/local/tomcat/webapps/HelloWorld.war
EXPOSE 8080
