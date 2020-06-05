FROM tomcat:7-jre8

# Set Environment Variables.
ENV FCREPO_VERSION=4.7.2 \
    FCREPO_HOME=/opt/fcrepo/data \
    FCREPO_PORT=8984

# Setup Fedora Data Volume.
RUN mkdir -p ${FCREPO_HOME}
VOLUME ${FCREPO_HOME}

# Download the Fedora WAR.
RUN curl -L https://repo1.maven.org/maven2/org/fcrepo/fcrepo-webapp/${FCREPO_VERSION}/fcrepo-webapp-${FCREPO_VERSION}.war > ${CATALINA_HOME}/webapps/fcrepo.war

# Copy the Server config.
COPY conf/server.xml ${CATALINA_HOME}/conf/server.xml
COPY conf/repository.json ${CATALINA_HOME}/conf/repository.json

# Set Java options for Tomcat and Fedora.
ENV JAVA_OPTS='-Dfcrepo.home=${FCREPO_HOME} -Dfcrepo.modeshape.configuration=file:${CATALINA_HOME}/conf/repository.json -Dfcrepo.mysql.host=fcrepo_db -Dfcrepo.mysql.port=3306 -Dfcrepo.mysql.username=root -Dfcrepo.mysql.password=root'

EXPOSE 8984
