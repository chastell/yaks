require 'yaks'
require 'yaks-html'
require 'rubygems/package_task'
require 'rspec/core/rake_task'
require 'yard'

def delegate_task(gem, task_name)
  task task_name do
    chdir gem.to_s do
      sh "rake", task_name.to_s
    end
  end
end

[:yaks, :"yaks-html", :"yaks-sinatra"].each do |gem|
  namespace gem do
    desc 'Run rspec'
    delegate_task gem, :rspec

    desc 'Build gem'
    delegate_task gem, :gem

    desc 'Generate YARD docs'
    delegate_task gem, :yard

    desc 'push gem to rubygems'
    task :push => "#{gem}:gem" do
      sh "gem push pkg/#{gem}-#{Yaks::VERSION}.gem"
    end
  end
end

task :tag do
  sh "git tag v#{Yaks::VERSION}"
  sh "git push --tags"
end

desc "Push gem to rubygems.org"
task :push_all => [
       :tag,
       "yaks:gem",
       "yaks-html:gem"
       "yaks-sinatra:gem",
       "yaks:push",
       "yaks-html:push"
       "yaks-sinatra:push"
     ]

desc "Run all the tests"
task :rspec => ["yaks:rspec", "yaks-html:rspec"]

desc 'Run mutation tests'
delegate_task :yaks, :mutant
