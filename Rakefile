require 'rubygems' unless defined?(Gem)
require 'rspec/core/rake_task'

specs = Dir['./spec/**/*_spec.rb']

specs.each do |spec|
  RSpec::Core::RakeTask.new("spec:#{File.basename(spec, '_spec.rb')}") do |t|
    t.pattern = spec
    t.rspec_opts = %w(-fs --color --fail-fast)
  end
end

desc "Run complete application spec suite"
RSpec::Core::RakeTask.new("spec") do |t|
  t.pattern = './spec/**/*_spec.rb'
  t.rspec_opts = %w(-fs --color --fail-fast)
end

desc "Launch a single app"
task :launch, :app do |t, args|
  raise "Please specify an app=padrino_basic !" unless args.app
  Bundler.require(:debug, :padrino)
  begin
    app = File.expand_path("../fixtures/single-apps/#{args.app}.rb", __FILE__)
    app_was = File.read(app)
    require app
    Padrino.run!
  ensure
    File.open(app, "w") { |f| f.write app_was }
  end
end

# TRAVIS handle concurrency rubies, so single_apps are not edited correctly.
task :default => 'spec:padrino'
