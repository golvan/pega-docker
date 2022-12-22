# escape=`

FROM busybox AS builder

# Expand prweb to target directory
COPY lib/prweb.war /prweb.war
RUN mkdir prweb
RUN unzip -q -o prweb.war -d /prweb


FROM pegasystems/pega-ready

# Import prweb to tomcat webapps directory
COPY --chown=pegauser:root --from=builder /prweb ${CATALINA_HOME}/webapps/prweb

RUN chmod -R g+rw ${CATALINA_HOME}/webapps/prweb

# Make a jdbc driver available to tomcat applications
COPY --chown=pegauser:root lib/postgresql-42.5.0.jar ${CATALINA_HOME}/lib/
