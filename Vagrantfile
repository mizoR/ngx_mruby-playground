# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.vm.box = 'centos64-x86_64'
  config.vm.box_url = 'https://github.com/2creatives/vagrant-centos/releases/download/v6.4.2/centos64-x86_64-20140116.box'
  config.vm.provision 'shell', inline: <<-EOS.gsub(/^    /, '')
    yum -y update
    yum -y install epel-release gcc openssl-devel libyaml-devel readline-devel zlib-devel git wget pcre-devel
    test -d /opt/rbenv || git clone git://github.com/sstephenson/rbenv.git /opt/rbenv
    mkdir -p /opt/rbenv/plugins
    test -d /opt/rbenv/plugins/ruby-build || git clone https://github.com/sstephenson/ruby-build.git /opt/rbenv/plugins/ruby-build
    cat > /etc/profile.d/rbenv.sh << "RBENV"
    export PATH="/opt/rbenv/bin:$PATH"
    eval "$(rbenv init -)"
    RBENV
    source /etc/profile.d/rbenv.sh
    (rbenv versions | grep -q 2.2.2) && (echo Package rbenv already installed; echo Nothing to do) || rbenv install 2.2.2
    rbenv global 2.2.2
    mkdir -p /usr/local/src
    test -d /usr/local/src/ngx_mruby || git clone https://github.com/matsumoto-r/ngx_mruby.git /usr/local/src/ngx_mruby
    (cd /usr/local/src/ngx_mruby && git pull origin master && git submodule init && git submodule update)
    (cd /usr/local/src/ngx_mruby && NGINX_CONFIG_OPT_ENV='--prefix=/usr/local/nginx' sh build.sh)
    (cd /usr/local/src/ngx_mruby && make install)
  EOS
end
