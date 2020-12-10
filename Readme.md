### Alpine Docker containers for Elixir
A single CI pipeline to build Erlang / Elixir / Phoenix Docker containers.



### Usage
All relevant information is kept in a single file: `bin/env-vars`.

Following ENV variables are available:

```
export REFRESHED_AT="2020-12-10"
export DOCKERHUB_USER="mindreframer"

export ELIXIR_VERSION="1.11.1"
export ELIXIR_IMAGE="alpine-elixir"

export ERLANG_VERSION="23.1.2"
export ERLANG_IMAGE="alpine-erlang"
```
