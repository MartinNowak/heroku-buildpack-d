# Heroku Buildpack: D

A heroku buildpack for the D programing language using dub to build applications.

# Usage

Example usage:

    $ ls
    dub.sdl source .d-compiler

    $ heroku create --buildpack http://github.com/MartinNowak/heroku-buildpack-d.git

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom git buildpack... done
    -----> D app detected
    -----> Using dmd-2.068.2
    -----> Using dub-0.9.24
    -----> Building application with dub

# Usage with Vibe.d
Add `Procfile` to your project and edit content.

`web: ./your-app-name`

Also reconfigure vibe HTTPServerSettings. For example;

```
auto settings = new HTTPServerSettings;
settings.port = to!ushort(environment.get("PORT", "8080"));
settings.bindAddresses = ["0.0.0.0", "127.0.0.1"];
...
```

# Selecting a D compiler

By default the latest dmd compiler is used. It is also possible to use gdc or
ldc and to choose a specific compiler versions by adding a `.d-compiler` file to
your project. Use `dmd`, `ldc`, or `gdc` to select the latest or `dmd-2.068.2`,
`ldc-0.16.0`, or `gdc-4.9.2` to select a specific version of a compiler.
