FROM alpine:3.5
RUN apk --update add ruby ruby-dev ruby-json build-base && gem install bundler --no-ri --no-rdoc

ENV APP_HOME /app
RUN mkdir $APP_HOME
WORKDIR $APP_HOME
ADD Gemfile* $APP_HOME/
RUN bundle install
ADD . $APP_HOME
ENV POST_SERVICE_HOST post
ENV POST_SERVICE_PORT 5000
ENV COMMENT_SERVICE_HOST comment-ui
ENV COMMENT_SERVICE_PORT 9292
CMD ["puma"]