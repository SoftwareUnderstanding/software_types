# Software Types

Schema.org profile for software types, used for software metadata descriptions

Authors: Maarten van Gompel and Daniel Garijo

Profile available at: https://w3id.org/software-types

## Introduction

This profile offers additional vocabulary needed to describe software metadata.
It specifically focuses on encoding the software type. It is meant to be used
with schema.org and [codemeta](https://codemeta.github.io). We aim to introduce
as little additional vocabulary as possible and only extend where schema.org
and codemeta leave gaps.

## Why do we need a profile for software types?

A software application typically offers one or more interfaces through which
users or machines can interact with it. This information is already partially
available in schema.org as there are subtypes of ``schema:SoftwareApplication``
like ``schema:WebApplication``, ``schema:MobileApplication``, and even
``schema:VideoGame``. We recommend their usage and in the ``software_types``
profile developed here attempt to fill the gaps and define further (sub)types
such as command-line applications, desktops applications (GUIs) and programming
libraries.

This availability of explicit software interface type information allows for
more accurate software metadata descriptions. Implementing these as types, in
line with what schema.org already does, allows for further type-specific
refinements. A good example is the
[WebAPI](https://github.com/schemaorg/schemaorg/issues/1423) type that is in
development which allows for descripting Web APIs, though not a subtype of
``schema:SoftwareApplication``.

We define the following additional vocabulary to describe software based on
interface type, most are a subclass of `schema:SoftwareApplication`.

* ``CommandLineApplication`` - A software application offering a command-line interface
* ``DesktopApplication`` - A software application offering a graphical user interface on the desktop
* ``TerminalApplication`` - A software application offering a terminal text-based user interface (e.g. ncurses)
* ``SoftwareLibrary`` - A software application offering an Application Programming Interface (API) for developers.
* ``SoftwareImage`` - A software application in the form of an image (such as a container image or virtual machine image), distributes the application along with its wider dependency context.
* ``SoftwarePackage`` - A software application in the form of a package for any particular package manager, distributes the application but not its wider dependency context.
* ``NotebookApplication`` - A web application in the form of a notebook (e.g. Jupyter Notebook, R Notebook) or data story.

## How are these software types used with codemeta?

Codemeta, building upon schema.org, describes software metadata focused on the
software's source code (`schema:SoftwareSourceCode`). We call the various
artefacts that can be produced by the source code **target products** and use
use the existing `schema:targetProduct` property to link the source code
description to one or more target products (see discussion
[here](https://github.com/codemeta/codemeta/issues/267)). These target products
take one of the types defines in our profile, or one of the existing ones
already in schema.org.

Example A (JSON-LD):

```json
{
    "@context": [
        "https://raw.githubusercontent.com/codemeta/codemeta/2.0/codemeta.jsonld",
        "https://raw.githubusercontent.com/schemaorg/schemaorg/main/data/releases/13.0/schemaorgcontext.jsonld",
        "https://w3id.org/software-types"
    ],
    "@type": "SoftwareSourceCode",
    "name": "MyTool",
    "codeRepository": "https://github.com/someuser/mytool",
    ...,
    "targetProduct": [
        {
            "type": "CommandLineApplication",
            "executableName": "mytool",
            "name": "MyTool",
            "runtimePlatform": "Linux"
        },
        {
            "type": "SoftwareLibrary",
            "executableName": "libmytool",
            "name": "MyTool Library",
            "runtimePlatform": "Linux"
        },
    ]
}
```

Example B:

```json
{
    "@context": [
        "https://raw.githubusercontent.com/codemeta/codemeta/2.0/codemeta.jsonld",
        "https://raw.githubusercontent.com/schemaorg/schemaorg/main/data/releases/13.0/schemaorgcontext.jsonld",
        "https://w3id.org/software-types"
    ],
    "@type": "SoftwareSourceCode",
    "name": "MyTool",
    "codeRepository": "https://github.com/someuser/mytool",
    ...,
    "targetProduct": [
        {
            "type": "WebApplication",
            "executableName": "mytool-webapp",
            "provider": {
                "@type": "Organization",
                "name": "Some hosting party"
            }
            "url": "https://somewhere.org/mytool-service"
        },
        {
            "type": "WebAPI",
            "provider": {
                "@type": "Organization",
                "name": "Some hosting party"
            }
            "endpointUrl": {
                "@type": "EntryPoint",
                "url": "https://somewhere.org/mytool-service",
                "contentType": "application/json"
            },
            "endpointDescription": {
                {
                  "@type": "CreativeWork",
                  "encodingFormat": "application/json",
                  "url": "https://somewhere.org/mytool-service/info?version=v1"
                },
            },
            "conformsTo": "https://jsonapi.org/format/1.0/",
            "documentation": "https://somewhere.org/mytool-service/docs",
        },
    ]
}
```

## Software as a Service 

The vocabulary we propose here allows to express a link between software
metadata and instances of that software running as a service somewhere. In
example B you see ``schema:WebAPI`` and ``schema:WebApplication`` used to
describe instances of software as a service. We use ``schema:WebAPI`` with a
proposed extension as formulated
[here](https://webapi-discovery.github.io/rfcs/rfc0001.html) and discussed
[here](https://github.com/schemaorg/schemaorg/issues/1423), nothing in there is
defined by us.

The link between `SoftwareSourceCode` and subclasses of `SoftwareApplication`,
`WebAPI`, `WebPage` or `WebSite` is established using the `targetProduct`
property. There is also a reverse property for this called
``codemeta:hasSourceCode`` (see discussion
[here](https://github.com/codemeta/codemeta/pull/229)).

## Additional Properties

### Executable name

In order to explicitly encode the name of a certain software executable, we
define a `executableName` property, shown in examples A and B. This is to
be interpreted as the name of the executable within a certain undefined
run-time context (e.g. an executable filename or name of an importable module).
It should typically not contain any platform/runtime-specific extensions (``.exe``,``.so``,``.dll``,``.dylib``,``.py``, ``.sh``).

## Implementations

Support for our `software_types` extension to codemeta/schema.org is
implemented in the following software:

* [codemetapy](https://github.com/proycon/codemetapy) - Python library and command-line tool for converting to codemeta and creating/manipulating existing codemeta descriptions.
* [codemeta-harvester](https://github.com/proycon/codemeta-harvester) - Automatically harvests and converts software metadata to codemeta

## Mapping existing work

TODO: Explain how we map other work like cwl, hydra, doap, etc. Many works exist for similar purposes!

## Acknowledgement

This work was indirectly and partially funded through the [CLARIAH-PLUS project](https://clariah.nl).
