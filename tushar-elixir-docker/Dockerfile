FROM erlang:18

# elixir expects utf8.
ENV ELIXIR_VERSION="v1.2.6" \
	LANG=C.UTF-8

RUN set -xe \
	&& ELIXIR_DOWNLOAD_URL="https://github.com/elixir-lang/elixir/releases/download/${ELIXIR_VERSION}/Precompiled.zip" \
	&& buildDeps=' \
		unzip \
	' \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends $buildDeps \
	&& curl -fSL -o elixir-precompiled.zip $ELIXIR_DOWNLOAD_URL \
	&& unzip -d /usr/local elixir-precompiled.zip \
	&& rm elixir-precompiled.zip \
	&& apt-get purge -y --auto-remove $buildDeps \
	&& rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install telnet net-tools

ADD poxa /poxa

RUN cd /poxa \
	&& yes | MIX_ENV=prod mix do deps.get, compile, release --force

WORKDIR /poxa

EXPOSE 8080

CMD /poxa/rel/poxa/bin/poxa start;bash