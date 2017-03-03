FROM penkit/ruby:1.9.3

COPY Gemfile .

RUN set -ex; \
  apk add --no-cache \
    libxml2 \
    nodejs \
    sqlite-libs \
    tzdata \
    yaml \
    zlib \
    ; \
  apk add --no-cache --virtual .bundle-deps \
    build-base \
    libxml2-dev \
    ruby-dev=$RUBY_VERSION \
    sqlite-dev \
    zlib-dev \
    ; \
  bundle install; \
  mkdir -p tmp/cache/assets; \
  chown -R ruby:ruby tmp; \
  apk del .bundle-deps; \
  rails new . --skip-gemfile --skip-bundle

COPY db ./db
COPY app ./app
COPY config ./config
COPY Rakefile .
COPY config.ru .

RUN bundle exec rake db:migrate

EXPOSE 8080

CMD ["bundle", "exec", "rails", "s", "--port", "8080", "-b", "0.0.0.0"]