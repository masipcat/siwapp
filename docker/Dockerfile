FROM masipcat/elixir:1.13.4-otp-24.2

ENV PHOENIX_VERSION 1.6.2

# Skip post-install steps
ENV DEBIAN_FRONTEND=noninteractive

# Install required packages
RUN apt-get update -qq && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        git \
        gnupg \
        inotify-tools \
        wget && \
    wget --no-check-certificate \
        -qO chrome.deb \
        https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && \
    apt-get -y --no-install-recommends install ./chrome.deb && \
    rm chrome.deb && \
    wget -O /tmp/node.sh https://deb.nodesource.com/setup_16.x && \
    bash /tmp/node.sh && rm /tmp/node.sh && \
    apt-get install nodejs -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Create required dirs
RUN mkdir -p /app /home/ubuntu

# Set user permissions
ARG user_id
ENV user_id $user_id

RUN chown $user_id /app /home/ubuntu
USER $user_id
ENV HOME /home/ubuntu

# Install hex and rebar
RUN mix local.hex --force
RUN mix local.rebar --force

# Install phoenix
RUN mix archive.install hex phx_new $PHOENIX_VERSION

# Set /app as workdir
WORKDIR /app
