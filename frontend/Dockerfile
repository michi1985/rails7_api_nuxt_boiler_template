FROM node:18 as node
FROM ruby:3.2.2

RUN apt-get update -qq \
    && apt-get install -y --no-install-recommends \
                    build-essential \
                    ca-certificates \
                    graphviz \
    && rm -rf /var/lib/apt/lists/*

RUN curl -sL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install nodejs

COPY --from=node /opt/yarn-* /opt/yarn
COPY --from=node /usr/local/bin/node /usr/local/bin/

RUN ln -s /opt/yarn/bin/yarn /usr/local/bin/yarn \
    && ln -s /opt/yarn/bin/yarnpkg /usr/local/bin/yarnpkg

WORKDIR /usr/src

COPY ./backend/Gemfile /usr/src/backend/Gemfile
COPY ./backend/Gemfile.lock /usr/src/backend/Gemfile.lock
RUN cd /usr/src/backend && bundle install

COPY . .

CMD ["/bin/sh", "-c", "rm -f tmp/pids/server.pid && cd backend && bundle exec rails s -p 3000 -b '0.0.0.0'"]
