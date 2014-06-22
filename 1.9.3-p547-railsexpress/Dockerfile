FROM ubuntu:14.04
MAINTAINER Brian Morton "bmorton@yammer-inc.com"

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update
RUN apt-get install -y curl ca-certificates git build-essential libtool autoconf libtcmalloc-minimal4

# Install chruby
ADD https://github.com/postmodern/chruby/archive/v0.3.8.tar.gz /tmp/chruby.tar.gz
RUN cd /tmp && tar -xzvf chruby.tar.gz
RUN cd /tmp/chruby-0.3.8 && make install

# Install ruby-install
ADD https://github.com/postmodern/ruby-install/archive/v0.4.3.tar.gz /tmp/ruby-install.tar.gz
RUN cd /tmp && tar -xzvf ruby-install.tar.gz
RUN cd /tmp/ruby-install-0.4.3 && make install

RUN ruby-install \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/01-fix-make-clean.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/02-railsbench-gc.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/03-display-more-detailed-stack-trace.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/04-fork-support-for-gc-logging.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/05-track-live-dataset-size.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/06-webrick_204_304_keep_alive_fix.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/07-export-a-few-more-symbols-for-ruby-prof.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/08-thread-variables.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/09-faster-loading.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/10-falcon-st-opt.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/11-falcon-sparse-array.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/12-falcon-array-queue.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/13-railsbench-gc-fixes.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/14-show-full-backtrace-on-stack-overflow.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/15-configurable-fiber-stack-sizes.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/16-backport-psych-20.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/17-fix-missing-c-return-event.patch \
  -p https://raw.github.com/skaes/rvm-patchsets/master/patches/ruby/1.9.3/p547/railsexpress/18-fix-process-daemon-call.patch \
  ruby 1.9.3-p547 -- --disable-install-rdoc --enable-shared CFLAGS="-O3"

RUN echo '[ -n "$BASH_VERSION" ] || [ -n "$ZSH_VERSION" ] || return' >> /etc/profile.d/chruby.sh
RUN echo 'source /usr/local/share/chruby/chruby.sh' >> /etc/profile.d/chruby.sh
RUN echo 'chruby ruby' >> /etc/profile.d/default_ruby.sh
RUN echo "export LD_PRELOAD=/usr/lib/libtcmalloc_minimal.so.4:${LD_PRELOAD}" >> /etc/profile.d/exports.sh

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
