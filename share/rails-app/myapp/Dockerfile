FROM ruby:2.5.3

ENV APP_DIR /usr/src/myapp

RUN mkdir -p ${APP_DIR}

WORKDIR $APP_DIR

RUN curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs mysql-client &&\
    npm install -g yarn

COPY . $APP_DIR
RUN bundle install --path vendor/bundle
CMD ["bundle", "exec", "rails", "server", "-b", "0.0.0.0"]
