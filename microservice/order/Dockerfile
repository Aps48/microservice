FROM maven:3.8.3-openjdk-17 AS builder
# FROM us.icr.io/atg-devsecops-support/openjdk-maven:17-3.6.3 as builder
WORKDIR /app
COPY pom.xml /app/pom.xml
RUN mvn clean
COPY . /app
RUN mvn clean install -Dmaven.test.skip=true

#FROM us.icr.io/atg-devsecops-support/openjdk:17
#RUN groupadd -r atguser && useradd -r -g atguser atguser
#USER atguser
#VOLUME /tmp
#COPY --from=builder /app/target/Slack-App-Interactivity-Service-*.jar app.jar
#EXPOSE 8080
#CMD java -Djava.security.egd=file:/dev/./urandom -jar app.jar

FROM open-liberty:25.0.0.1-full-java17-openj9
# FROM us.icr.io/atg-devsecops-support/cirrus-open-liberty:23.0.0.9-full-java17-openj9-ubi
USER 1001
COPY --from=builder --chown=1001:0 --chmod=775 /app/target/*.jar /config/apps/app.jar
#COPY --chown=1001:0 --chmod=775  src/main/liberty/config/server.xml /config/server.xml
EXPOSE 8081
ENTRYPOINT ["java", "-jar", "config/apps/app.jar"]
