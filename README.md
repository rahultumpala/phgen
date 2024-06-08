# phgen

Portable Http GENerator. A CLI tool to generate and intercept HTTP Requests.

I want a command line tool that allows me to generate 1000s of HTTP requests to load test an app in my machine, or in another machine in the network. In addition, I want to intercept HTTP requests made from my machine and fiddle with the response.

phgen is a tool that serves this purpose.

Syntax
-----

phgen uses a simple scripting language to build the underlying mechanism.

Syntax Guide for the phgen scripting language is available [here](docs/Syntax.md)

Grammar of the DSL in extended Backur-Naur form (EBNF) is available [here](docs/phgen_ebnf.txt)


Features Roadmap
-------
- Generate HTTP Requests to a given destination
  - Synchronize with other phgen nodes in the network and generate in sync
- Generate HTTP listeners matching a request definition and serve static content
- Intercept HTTP requests matcing a request definition (Act as a Proxy)

_~rahultumpala_
