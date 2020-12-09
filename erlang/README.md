# Erlang on Alpine Linux

This Dockerfile provides a full installation of Erlang on Alpine, intended for running Erlang releases,
so it has no build tools installed. The Erlang installation is provided so one can avoid cross-compiling
releases. The caveat of course is if one has NIFs which require a native compilation toolchain, but that is
left as an exercise for the reader.

**NOTE:** This image is primarily intended to either be used as-is, or as the base for an image which is
embellished with additional dependencies. It is also intended to be forked and tweaked as needed for those
applications which require different build configurations of OTP. If the default configuration does not work
for you, you will need to take one of the approaches described under _Extending for your own application_.

## Usage

NOTE: This image sets up a `default` user, with home set to `/opt/app` and owned by that user. The working directory
is also set to `$HOME`. It is highly recommended that you add a `USER default` instruction to the end of your
Dockerfile so that your app runs in a non-elevated context.

To boot straight to a prompt in the image (versioning info is stripped here, but this is just to give you a general idea
of what to expect):

```
$ docker run --rm -it --user=root bitwalker/alpine-erlang erl
Erlang/OTP XX [erts-X.X] [source] [64-bit] [smp:4:4] [async-threads:10] [hipe] [kernel-poll:false]

Eshell VX.X  (abort with ^G)
1>
BREAK: (a)bort (c)ontinue (p)roc info (i)nfo (l)oaded
       (v)ersion (k)ill (D)b-tables (d)istribution
a
```

## Extending for your own application

**NOTE:** The dependency requirements for your own application may need additional system packages installed via APK,
or additional OTP applications from the standard library which are not built by default in order to save space. In
the former case, you need to build your own image based on `alpine-erlang` which installs those extra packages. If
you need additional OTP applications from the standard library, you will need to fork `alpine-erlang` and tweak the
config flags for the build so that those applications are present.

```dockerfile
FROM bitwalker/alpine-erlang:latest

# Set exposed ports
EXPOSE 5000
ENV PORT=5000

ENV MIX_ENV=prod

ADD yourapp.tar.gz ./

USER default

ENTRYPOINT ["./bin/yourapp"]
CMD ["foreground"]
```

## License

MIT
