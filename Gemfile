# frozen_string_literal: true

source "https://rubygems.org"

gem "jekyll-theme-chirpy", "~> 7.2", ">= 6.3.1"

group :test do
  gem "html-proofer", "~> 4.4"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]

# Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds since newer versions of the gem
# do not have a Java counterpart.
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]


# Gem i included to help generate posts
gem 'jekyll-compose', group: [:jekyll_plugins]

gem 'jekyll-redirect-from'

gem 'jekyll-sitemap'

gem 'google-protobuf', '>= 3.25.1'

gem 'logger'
gem 'csv'
gem 'base64'
gem 'ostruct'
gem 'ffi', '~> 1.15.0' 