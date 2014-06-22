FROM ubuntu:14.04
MAINTAINER Brian Morton "bmorton@yammer-inc.com"

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update
RUN apt-get install -y curl ca-certificates git build-essential libtool autoconf libtcmalloc-minimal4 ruby

# Install chruby
ADD https://github.com/postmodern/chruby/archive/v0.3.8.tar.gz /tmp/chruby.tar.gz
RUN cd /tmp && tar -xzvf chruby.tar.gz
RUN cd /tmp/chruby-0.3.8 && make install

# Install ruby-install
ADD https://github.com/postmodern/ruby-install/archive/v0.4.3.tar.gz /tmp/ruby-install.tar.gz
RUN cd /tmp && tar -xzvf ruby-install.tar.gz
RUN cd /tmp/ruby-install-0.4.3 && make install

RUN ruby-install \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/2.1.2/railsexpress/01-zero-broken-tests.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/2.1.2/railsexpress/02-improve-gc-stats.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/2.1.2/railsexpress/03-display-more-detailed-stack-trace.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/2.1.2/railsexpress/04-show-full-backtrace-on-stack-overflow.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/2.1.2/railsexpress/05-fix-missing-c-return-event.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/2.1.2/railsexpress/06-backport-006e66b6680f60adfb434ee7397f0dbc77de7873.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/2.1.2/railsexpress/07-funny-falcon-stc-density.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/2.1.2/railsexpress/08-funny-falcon-stc-pool-allocation.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/2.1.2/railsexpress/09-aman-opt-aset-aref-str.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/2.1.2/railsexpress/10-funny-falcon-method-cache.patch \
  ruby 2.1.2 -- --disable-install-rdoc --enable-shared CFLAGS="-O3"

RUN echo '[ -n "$BASH_VERSION" ] || [ -n "$ZSH_VERSION" ] || return' >> /etc/profile.d/chruby.sh
RUN echo 'source /usr/local/share/chruby/chruby.sh' >> /etc/profile.d/chruby.sh
RUN echo 'chruby ruby' >> /etc/profile.d/default_ruby.sh
RUN echo "export LD_PRELOAD=/usr/lib/libtcmalloc_minimal.so.4:${LD_PRELOAD}" >> /etc/profile.d/exports.sh

# Clean up APT when done.
RUN apt-get remove -y ruby
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN chruby-exec ruby -- gem install bundler
