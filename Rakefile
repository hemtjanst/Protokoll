require 'colorize'
require 'html-proofer'
require 'jekyll'

task :default => :test

desc 'Build the site with Jekyll'
task :build do
  Jekyll::Commands::Build.process(profile: true)
end

desc 'Remove generated site'
task :clean do
  Jekyll::Commands::Clean.process({})
end

desc 'Validate _site/ with html-proofer'
task :validate do
  HTMLProofer.check_directory('./_site', {
    :directory_index_file => "index.html",
    :internal_domains => ["hemtjan.st"],
    :check_html => true,
    :assume_extension => true,
    :url_swap => { "\/protokoll\/" => "\/" }, # Needed b/c of baseurl
  }).run
end

desc 'Check for Jekyll deprecation issues'
task :doctor do
  Jekyll::Commands::Doctor.process({})
end

desc 'Build and validate the site'
task :test do
  notify 'Building site'
  Rake::Task['build'].invoke
  notify 'Validating site'
  Rake::Task['validate'].invoke
  notify 'Checking for deprecation issues'
  Rake::Task['doctor'].invoke
end

def notify message
  puts
  puts '###################################################'.blue
  puts "#{message}...".blue
  puts '###################################################'.blue
  puts
  STDOUT.flush
end
