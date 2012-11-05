require 'jasmine-phantom/server'

begin
  require 'jasmine'
  load 'jasmine/tasks/jasmine.rake'
rescue LoadError
  task :jasmine do
    abort "Jasmine is not available. In order to run jasmine, you must: (sudo) gem install jasmine"
  end
end

namespace :jasmine do
  namespace :phantom do
    desc "Run jasmine specs using phantomjs and report the results"
    task :ci => "jasmine:require" do
      jasmine_config_overrides = File.join(Jasmine::Config.new.project_root, 'spec', 'javascripts' ,'support' ,'jasmine_config.rb')
      require jasmine_config_overrides if File.exist?(jasmine_config_overrides)

      port = Jasmine::Phantom::Server.start
      script = File.join File.dirname(__FILE__), 'spec/javascripts/support/run-jasmine.js'

      pid = Process.spawn "phantomjs #{script} http://localhost:#{port}"

      begin
        Thread.pass
        sleep 0.1
        wait_pid, status = Process.waitpid2 pid, Process::WNOHANG
      end while wait_pid.nil?
      exit(1) unless status.success?
    end
  end
end
