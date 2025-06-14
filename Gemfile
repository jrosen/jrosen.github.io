source 'https://rubygems.org'

# Update Jekyll to a newer version that's compatible with Ruby 3.2
gem 'jekyll', '~> 4.3.3'

# Add required dependencies
gem 'webrick', '~> 1.8'  # Required for Ruby 3.0+
gem 'rexml', '~> 3.2.5'  # Required for Ruby 3.0+
gem 'kramdown', '~> 2.4.0'  # Updated kramdown for Ruby 3.2 compatibility

# Theme plugins
gem 'jekyll-paginate-v2', '~> 3.0'
gem 'jekyll-feed', '~> 0.17.0'
gem 'jekyll-sitemap', '~> 1.4.0'
gem 'jekyll-seo-tag', '~> 2.8.0'
gem 'jekyll-gist', '~> 1.5.0'
gem 'jekyll-relative-links', '~> 0.7.0'

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem 'tzinfo', '>= 1', '< 3'
  gem 'tzinfo-data'
  gem 'http_parser.rb', '~> 0.6.0'  # Required for JRuby
end

# Performance-booster for watching directories on Windows
gem 'wdm', '~> 0.1.1', :platforms => [:mingw, :x64_mingw, :mswin]