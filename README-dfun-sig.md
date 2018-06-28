# Docker Fundamentals Signature Assignement

The signature application is a short exercise that should take no longer than a few hours. Unfortunately, we never have enough time in class to full go through the exercise.  I hope you can spare enough time to give the application a shot, as it does replicate a real world applicatin quite well.

The first step is to clone the got repository locally.

'''sh
git clone -b ee2.0 https://github.com/docker-training/fundamentals-final.git
'''

You will find three directories in ths bundle, api, database and ui.

The task is to create a Dockerfile for each, as well as a docker-compose.-yaml to put them all together into a service or stack that will accomplish the stated goals.



api/Dockerfile

```bash
FROM maven:latest AS appserver
COPY . .
RUN mvn -B -f pom.xml -s /usr/share/maven/ref/settings-docker.xml dependency:resolve
RUN mvn -B -s /usr/share/maven/ref/settings-docker.xml package -DskipTests
FROM java:8-jdk-alpine
RUN adduser -Dh /home/gordon gordon
COPY --from=appserver /target/ddev-0.0.1-SNAPSHOT.jar .
ENTRYPOINT ["java", "-jar", "/ddev-0.0.1-SNAPSHOT.jar"]
CMD ["--spring.profiles.active=postgres"]
```


database/Dockerfile

```bash
FROM postgres:9.6

ENV  POSTGRES_USER gordonuser
ENV  POSTGRES_PASSWORD gordonpass
ENV  POSTGRES_DB ddev

COPY pg_hba.conf /usr/share/postgresql/9.6
COPY postgresql.conf /usr/share/postgresql/9.6
RUN  mkdir -p /docker-entrypoint-initdb.d
COPY init-db.sql /docker-entrypoint-initdb.d/

EXPOSE 5432
```

ui/Dockerfile

```bash
FROM node:8-alpine
COPY . .
RUN npm install
```

docker-compose.yaml

```yaml
version: "3.1"

services:
  database:
    image: ffdb
    networks:
    - back-end

  api:
    image: ffapi
    networks:
    - front-end
    - back-end
   
  ui:
    image: ffui
    ports:
    - "3000:3000"
    networks:
    - front-end

networks:
  front-end:
  back-end:
```
