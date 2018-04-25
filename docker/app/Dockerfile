# Image Base
FROM ruby:2.3.3
# Download Nodejs
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -
# Install packages
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs apt-transport-https
# Add Yarn repository (Only Rails >= 5)
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
# Install Yarn (Only Rails >= 5)
RUN apt-get update -qq && apt-get install -y yarn
# Create un directory for store application
RUN mkdir /app
# Directory for work
WORKDIR /app

# Sete environment variables
ENV RAILS_ENV='production'
ENV RAKE_ENV='production'
ENV RAILS_SERVE_STATIC_FILES = true
# Copy Gemfile & Gemfile.lock to directory remote
COPY Gemfile /app/Gemfile
COPY Gemfile.lock /app/Gemfile.lock
# Run bundle install ignoring environments development and test
RUN bundle install --jobs 20 --retry 5 --without development test
# Copy content application to container
COPY . /app
# Compile assets (Only Rails applications)
RUN bundle exec rake RAILS_ENV=production assets:precompile

# Expose port for work in other container
EXPOSE 3000

# Run server (Puma)
CMD ["bundle", "exec", "puma", "-C", "config/puma.rb"]
