require 'html-proofer'

task :default => :test

task :test do
  options = {
    :disable_external => true
  }
  sh 'bundle exec jekyll build'
  HTMLProofer.check_directory('./_site', options).run
end
