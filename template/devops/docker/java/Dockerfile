FROM java:8-jdk-alpine

COPY target/xxx.jar /usr/app/app.jar

WORKDIR /usr/app

ENTRYPOINT exec java $JAVA_OPTS -jar app.jar