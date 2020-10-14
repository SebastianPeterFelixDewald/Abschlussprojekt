# fluentd/Dockerfile

FROM fluent/fluentd:v1.11-debian-1
USER root
COPY conf/fluent.conf /fluentd/etc/fluent.conf
RUN ["gem", "install", "fluent-plugin-elasticsearch", "--no-document", "--version", "4.2.0"]
RUN ["gem", "install", "fluent-plugin-prometheus", "--no-document", "--version", "1.8.4"]
USER fluent