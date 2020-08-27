# Based on https://github.com/cypress-io/cypress-docker-images/blob/e0eb526/base/12.14.0/Dockerfile
FROM node:12.4.0

RUN apt-get update && \
  apt-get install --no-install-recommends -y \
  libgtk2.0-0 \
  libgtk-3-0 \
  libnotify-dev \
  libgconf-2-4 \
  libnss3 \
  libxss1 \
  libasound2 \
  libxtst6 \
  xauth \
  xvfb \
  # clean up
  && rm -rf /var/lib/apt/lists/*

RUN npm install -g npm@6.10.2
RUN npm install -g yarn@1.17.3

# a few environment variables to make NPM installs easier
# good colors for most applications
ENV TERM xterm
# avoid million NPM install messages
ENV npm_config_loglevel warn
# allow installing when the main user is root
ENV npm_config_unsafe_perm true

# versions of local tools
RUN echo  " node version:    $(node -v) \n" \
  "npm version:     $(npm -v) \n" \
  "yarn version:    $(yarn -v) \n" \
  "debian version:  $(cat /etc/debian_version) \n" \
  "user:            $(whoami) \n"

WORKDIR /app

COPY ./package.json ./yarn.lock ./

ENV CI=1

RUN yarn install --frozen-lockfile

RUN yarn run cypress verify
