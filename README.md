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
    -----> Using dmd-2.078.0
    -----> Building application with dub

## Usage with Vibe.d

The buildpack will infer a default [`Procfile`](https://devcenter.heroku.com/articles/procfile) for you, that should be sufficient for many cases.
```
default_process_types:
  web: ./your-app-name --port $PORT
```
Use [readOption](http://vibed.org/api/vibe.core.args/readOption) or [std.getopt](https://dlang.org/phobos/std_getopt.html#.getopt) to bind your HTTP server to the port choosen by Heroku.
Here is dub's vibe.d template (`dub init myapp --type=vibe.d`) modified accordingly.

```d
import vibe.vibe;

void main()
{
    auto settings = new HTTPServerSettings;
    settings.port = 8080;
    // <!-- NEW NEW NEW
    // listen on external interface by default
    settings.bindAddresses = ["0.0.0.0"];
    // read port from command line
    readOption("port|p", &settings.port, "Sets the port used for serving HTTP.");
    // optionally read bind address from command line
    readOption("bind-address|bind", &settings.bindAddresses[0], "Sets the address used for serving HTTP.");
    // NEW NEW NEW -->

    auto listener = listenHTTP(settings, &hello);
    runApplication();
}

void hello(HTTPServerRequest req, HTTPServerResponse res)
{
    res.writeBody("Hello, World!");
}
```

You can add a custom [`Procfile`](https://devcenter.heroku.com/articles/procfile) to your repo if you need more or different arguments.

`web: ./your-app-name --port $PORT -v --my-arg`

And finally see [Deploy on Heroku - Dlang Tour](https://tour.dlang.org/tour/en/vibed/deploy-on-heroku) for more details.

# Selecting a D compiler

By default the latest dmd compiler is used. It is also possible to use gdc or
ldc and to choose a specific compiler versions by adding a `.d-compiler` file to
your project or by setting the DC environment variable in Heroku. Use `dmd`,
`ldc`, or `gdc` to select the latest or `dmd-2.068.2`, `ldc-0.16.0`, or
`gdc-4.9.2` to select a specific version of a compiler.
