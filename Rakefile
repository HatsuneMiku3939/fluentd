#!/usr/bin/env rake
require "bundler/gem_tasks"

require 'fileutils'
require 'rake/testtask'
require 'rake/clean'

task test: [:base_test]

desc 'Run test_unit based test'
Rake::TestTask.new(:base_test) do |t|
  # To run test for only one file (or file path pattern)
  #  $ bundle exec rake base_test TEST=test/test_specified_path.rb
  #  $ bundle exec rake base_test TEST=test/test_*.rb
  t.libs << "test"
  t.test_files = if ENV["WIN_RAPID"]
                   ["test/test_event.rb", "test/test_supervisor.rb", "test/plugin_helper/test_event_loop.rb"]
                 else
                   Dir["test/**/test_*.rb"].sort
                 end
  t.verbose = true
  t.warning = true
  t.ruby_opts = ["-Eascii-8bit:ascii-8bit"]
end

task :parallel_test do
  FileUtils.rm_rf('./test/tmp')
  sh("parallel_test ./test/*.rb ./test/plugin/*.rb")
  FileUtils.rm_rf('./test/tmp')
end

desc 'Run test with simplecov'
task :coverage do |t|
  ENV['SIMPLE_COV'] = '1'
  Rake::Task["test"].invoke
end

task default: [:test, :build]
