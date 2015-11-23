require 'rubygems'
require 'bundler/setup'

require 'rake/clean'

distro = nil
fpm_opts = ""

if File.exist?('/etc/system-release') && File.read('/etc/redhat-release') =~ /centos|redhat|fedora|amazon/i
  distro = 'rpm'
  fpm_opts << " --rpm-user root --rpm-group root "
elsif File.exist?('/etc/os-release') && File.read('/etc/os-release') =~ /ubuntu|debian/i
  distro = 'deb'
  fpm_opts << " --deb-user root --deb-group root "
end

unless distro
  $stderr.puts "Don't know what distro I'm running on -- not sure if I can build!"
end

desc 'build git-crypt'
task :default do
 cd '/tmp' do
    sh("wget https://github.com/AGWA/git-crypt/archive/master.zip")
 end
 sh "unzip /tmp/master.zip"
 cd 'git-crypt-master' do
 sh("make all")
 sh("make install")
 end
end
