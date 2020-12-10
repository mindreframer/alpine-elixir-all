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

### Opt-in for container building
To prevent waiting for all containers in all commits, following magic strings are used in commit messages:
- `[skip ci]`
- `[with-erlang]`
- `[with-elixir]`
- `[with-phoenix]`

To trigger building all 3 containers, use `[with-erlang, with-elixir, with-phoenix]`.


### Secrets
Github Actions depend on following secrets being available:
- `DOCKERHUB_USER`
- `DOCKERHUB_TOKEN`
