# escape=`

FROM busybox AS builder

# Expand prweb to target directory
COPY lib/prweb.war /prweb.war
RUN mkdir prweb
RUN unzip -q -o prweb.war -d /prweb


FROM pegasystems/pega-ready

# Import prweb to tomcat webapps directory
COPY --chown=pegauser:root --from=builder /prweb ${CATALINA_HOME}/webapps/prweb

# Import tomcat manager to tomcat webapps directory
COPY --chown=pegauser:root --from=pegasystems/pega-ready ${CATALINA_HOME}/webapps.dist/manager ${CATALINA_HOME}/webapps/manager

RUN chmod -R g+rw ${CATALINA_HOME}/webapps/prweb
RUN chmod -R g+rw ${CATALINA_HOME}/webapps/manager

# Override default prconfig.xml
COPY --chown=pegauser:root conf/prconfig.xml /opt/pega/config/prconfig.xml
COPY --chown=pegauser:root conf/context.xml /opt/pega/config/context.xml
COPY --chown=pegauser:root conf/server.xml /opt/pega/config/server.xml
COPY --chown=pegauser:root conf/tomcat-users.xml /opt/pega/config/tomcat-users.xml
COPY --chown=pegauser:root conf/manager.xml ${CATALINA_HOME}/conf/Catalina/localhost/manager.xml

# Make a jdbc driver available to tomcat applications
COPY --chown=pegauser:root lib/postgresql-42.7.3.jar ${CATALINA_HOME}/lib/
