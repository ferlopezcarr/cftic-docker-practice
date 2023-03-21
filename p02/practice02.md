# Practice 02

Creamos un fichero Dockerfile, en este caso basado en Tomcat, vamos a instalar un HelloWorld. Como mi perfil es de Java voy a crear una instalación trivial con Tomcat, si prefieres hacerlo con una pila Python o PHP también es muy sencillo (con .NET no es tan trivial)

FROM tomcat
MAINTAINER raullapeira
RUN apt-get update && apt-get -y upgrade
WORKDIR /usr/local/tomcat
COPY HelloWorld.war /usr/local/tomcat/webapps/HelloWorld.war
EXPOSE 8080

Tendremos que copiar también el fichero war en la raíz de la instalación. 



Creamos la imagen, nos debería descargar primero las imágenes de las capas que forman la imagen de Tomcat y después construirla

PS C:\sw\imagenTomcat> docker build .
Sending build context to Docker daemon  839.2kB
Step 1/6 : FROM tomcat
 ---> f796d3d2c195
Step 2/6 : MAINTAINER raullapeira
 ---> Using cache
 ---> a3cc4ba7d9d2
Step 3/6 : RUN apt-get update && apt-get -y upgrade
 ---> Using cache
 ---> b2b6435b2fb8
Step 4/6 : WORKDIR /usr/local/tomcat
 ---> Using cache
 ---> b3aebbc34c63
Step 5/6 : COPY HelloWorld.war /usr/local/tomcat/webapps/HelloWorld.war
 ---> 3e280f4b5c26
Step 6/6 : EXPOSE 8080
 ---> Running in 3854e9d85457
Removing intermediate container 3854e9d85457
 ---> a4200b2a841b
Successfully built a4200b2a841b
SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.

A continuación lanzamos un nuevo contenedor a partir del id de la imagen

PS C:\sw\imagenTomcat> docker run a4200b2a841b
NOTE: Picked up JDK_JAVA_OPTIONS:  --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
03-Oct-2020 06:20:33.450 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version name:   Apache Tomcat/9.0.38
03-Oct-2020 06:20:33.491 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server built:          Sep 10 2020 08:20:30 UTC
03-Oct-2020 06:20:33.491 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log Server version number: 9.0.38.0
03-Oct-2020 06:20:33.491 INFO [main] org.apache.catalina.startup.VersionLoggerListener.log OS Name:               Linux
[...]
org.apache.jasper.servlet.TldScanner.scanJars At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time.
03-Oct-2020 06:20:37.092 INFO [main] org.apache.catalina.startup.HostConfig.deployWAR Deployment of web application archive [/usr/local/tomcat/webapps/HelloWorld.war] has finished in [1,356] ms
03-Oct-2020 06:20:37.096 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["http-nio-8080"]
03-Oct-2020 06:20:37.197 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [1888] milliseconds


No podemos acceder al contenedor por el 8080 porque no lo hemos mapeado al host al arrancar el contenedor ¿como habríamos expuesto y mapeado el puerto en el docker run?


docker run -p 8080:8080 a4200b2a841b

La aplicacion se despliega en una ruta como la siguiente

http://localhost:8080/helloworld 
