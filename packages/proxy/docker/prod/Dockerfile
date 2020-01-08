# -- Test

FROM python:slim
COPY nginx.conf .
RUN pip -q install gixy && gixy nginx.conf

# -- Build

FROM node:12-stretch
WORKDIR /opt/rss-proxy

SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN apt-get update \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* \
  && rm -rf /data/db \
  && mkdir -p /data/db

COPY [ "entrypoint.sh", ".build-cache/package.json", "./" ]

RUN yarn install --production

COPY .build-cache/* ./

RUN chown -R 0444 .

USER node

ENTRYPOINT [ "sh", "entrypoint.sh" ]

EXPOSE 8080