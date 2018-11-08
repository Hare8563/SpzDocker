FROM prepare-rails

WORKDIR /usr/src/rails-server
COPY Gemfile /usr/src/rails-server/Gemfile
COPY Gemfile.lock /usr/src/rails-server/Gemfile.lock
RUN bundle install
