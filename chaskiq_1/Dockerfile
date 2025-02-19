# ビルド用ステージ
FROM ruby:$RUBY_VERSION-slim-bullseye AS builder

ARG BUNDLER_VERSION
WORKDIR /usr/src/app

COPY Gemfile Gemfile.lock ./
RUN gem install bundler:$BUNDLER_VERSION && \
    bundle install --deployment --without development test

# 本番用ステージ
FROM ruby:$RUBY_VERSION-slim-bullseye

ARG APP_ENV
ENV RAILS_ENV=${APP_ENV}

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

COPY --from=builder /usr/src/app/vendor/bundle /usr/src/app/vendor/bundle
COPY . ./

COPY entrypoint.sh /usr/src/app/entrypoint.sh
RUN chmod +x /usr/src/app/entrypoint.sh

USER docker

ENTRYPOINT ["/usr/src/app/entrypoint.sh"]
CMD ["bundle", "exec", "rails", "server"]
