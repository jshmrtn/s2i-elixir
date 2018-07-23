FROM jshmrtn/s2i-erlang:21

ENV ERLANG_VERSION 21
ENV ELIXIR_VERSION 1.6.6

LABEL io.k8s.description="Platform for building and running an elixir app" \
      io.k8s.display-name="build-elixir" \
      io.openshift.tags="builder,erlang,elixir" \
      com.redhat.dev-mode="DEV_MODE:false" \
      com.redhat.deployments-dir="/opt/app-root/src" \
      name="jshmrtn/s2i-elixir" \
      maintainer="Jonatan Männchen <maennchen@joshmartin.ch>" \
      version="$ELIXIR_VERSION-otp-$ERLANG_VERSION"

USER 0

# Install Elixir
RUN set -x \
  && yum install -y --setopt=tsflags=nodocs \
    autoconf \
    epel-release \
    gcc \
    gcc-c++ \
    git \
    glibc-devel \
    make \
    ncurses-devel \
    openssl-devel \
    wxBase \
  && curl -LO "https://github.com/elixir-lang/elixir/archive/v${ELIXIR_VERSION}.tar.gz" \
  && tar xf v${ELIXIR_VERSION}.tar.gz \
  && rm -rf v${ELIXIR_VERSION}.tar.gz \
  && cd elixir-${ELIXIR_VERSION} \
  && make clean test \
  && make install \
  && cd .. \
  && rm -rf elixir-${ELIXIR_VERSION} \
  && yum remove -y \
    autoconf \
    epel-release \
    gcc-c++ \
    java-1.8.0-openjdk-devel \
    ncurses-devel \
    openssl-devel \
    wxBase \
  && yum clean all -y \
  && mix local.hex --force \
  && mix local.rebar --force


# Copy the S2I scripts from the image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Drop the root user and make the content of /opt/app-root owned by user 1001
RUN chown -R 1001:0 /opt/app-root && chmod -R ug+rwx /opt/app-root
USER 1001

ENV MIX_ENV prod

# Set the default CMD to print the usage of the language image
CMD $STI_SCRIPTS_PATH/usage
