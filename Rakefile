require "rubygems"
require "spec"
require "rake/clean"
require "rake/gempackagetask"
require "spec/rake/spectask"
require "pathname"

CLEAN.include "{log,pkg}/"

spec = Gem::Specification.new do |s|
  s.name             = "dm-polymorphic"
  s.version          = "1.1.0"
  s.platform         = Gem::Platform::RUBY
  s.has_rdoc         = true
  s.extra_rdoc_files = %w[ README.textile LICENSE TODO ]
  s.summary          = "DataMapper plugin enabling simple ActiveRecord style polymorphism"
  s.description      = s.summary
  s.author           = "Daniel Neighman, Wayne E. Seguin, Ripta Pasay, Martin Linkhorst, aldentea"
  s.email            = "has.sox@gmail.com, wayneeseguin@gmail.com, github@r8y.org, m.linkhorst@googlemail.com, aldentea-dev@otegami.aldente.mydns.jp"
  s.homepage         = "http://github.com/hassox/dm-polymorphic"
  s.require_path     = "lib"
  s.files            = FileList[ "{lib,spec}/**/*.rb", "spec/spec.opts", "Rakefile", *s.extra_rdoc_files ]
  s.add_dependency("dm-core", ">=#{s.version}")
end

task :default => [ :spec ]

WIN32 = (RUBY_PLATFORM =~ /win32|mingw|cygwin/) rescue nil
SUDO  = WIN32 ? "" : ("sudo" unless ENV["SUDOLESS"])

Rake::GemPackageTask.new(spec) do |pkg|
  pkg.gem_spec = spec
end

desc "Install #{spec.name} #{spec.version} (default ruby)"
task :install => [ :package ] do
  sh "#{SUDO} gem install pkg/#{spec.name}-#{spec.version} --no-update-sources", :verbose => false
end

namespace :jruby do
  desc "Install #{spec.name} #{spec.version} with JRuby"
  task :install => [ :package ] do
    sh %{#{SUDO} jruby -S gem install --local pkg/#{spec.name}-#{spec.version} --no-update-sources}, :verbose => false
  end
end

desc "Run specifications"
Spec::Rake::SpecTask.new(:spec) do |t|
  t.spec_opts << "--options" << "spec/spec.opts" if File.exists?("spec/spec.opts")
  t.spec_files = Pathname.glob(Pathname.new(__FILE__).dirname + "spec/**/*_spec.rb")

  begin
    t.rcov = ENV.has_key?("NO_RCOV") ? ENV["NO_RCOV"] != "true" : true
    t.rcov_opts << "--exclude" << "spec"
    t.rcov_opts << "--text-summary"
    t.rcov_opts << "--sort" << "coverage" << "--sort-reverse"
  rescue Exception
    # rcov not installed
  end
end
