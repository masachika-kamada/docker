FROM ubuntu:20.04
RUN apt-get update -y && \
  apt-get install -y nginx
ENV TEST_ENV="hage"
ENV APP_ENV="production"
CMD ["nginx", "-g", "daemon off;"]