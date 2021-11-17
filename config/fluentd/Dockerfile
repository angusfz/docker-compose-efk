FROM fluent/fluentd:v1.12.0-debian-1.0
USER root
RUN ["gem", "install", "fluent-plugin-elasticsearch", "--no-document", "--version", "5.0.3"]
RUN ["gem", "install", "fluent-plugin-s3", "--no-document", "--version", "1.6.0"]
RUN ["gem", "install", "fluent-plugin-rewrite-tag-filter", "--no-document", "--version", "2.4.0"]
USER fluent