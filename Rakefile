require 'rubygems'
require 'bundler/setup'

require 'rake/clean'

distro = nil
fpm_opts = ""
description_string = "git-crypt is used to encrypt the contents in VCS"
version = "0.5.0"


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
  jailed_root = File.expand_path('../jailed-root', __FILE__)
  rm_rf jailed_root
  mkdir_p jailed_root
  pkg = File.expand_path('../pkg', __FILE__)
  mkdir_p pkg
  prefix = "/opt/local/git-crypt"
  release = Time.now.utc.strftime('%Y%m%d%H%M%S')

  cd jailed_root do
    sh("wget https://github.com/AGWA/git-crypt/archive/master.zip")
  end
  cd jailed_root do
    sh ("unzip master.zip")
  end
  cd jailed_root do
    cd "git-crypt-master"
    sh("make all")
    sh("mkdir -p jailed_root/#{prefix}")
    sh("sudo make install PREFIX=#{prefix}")
    sh(%Q{
 bundle exec fpm -s dir -t #{distro} --name git-crypt -a x86_64 --version "#{version}" -C #{jailed_root} --verbose #{fpm_opts} --maintainer snap-ci@thoughtworks.com --vendor snap-ci@thoughtworks.com --url http://snap-ci.com --description "#{description_string}" --iteration #{release} --license 'Git-crypt' .
       })
    sh("mv git-crypt-#{version}-#{release}.x86_64.rpm ../../pkg")
  end
end
