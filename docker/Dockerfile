FROM ruby:3.2-slim

# Install system dependencies
RUN apt-get update -qq && apt-get install -y \
  build-essential \
  libpq-dev \
  curl \
  gnupg \
  git \
  nodejs \
  npm \
  sqlite3 \
  imagemagick \
  libvips \
  && rm -rf /var/lib/apt/lists/*

# Install Yarn
RUN npm install -g yarn

WORKDIR /app

COPY Gemfile* ./
RUN bundle install --jobs=4 --retry=3

COPY . .

RUN yarn install --frozen-lockfile
RUN RAILS_ENV=production bundle exec rake assets:precompile

EXPOSE 3000

CMD ["bundle", "exec", "puma", "-C", "config/puma.rb"]
