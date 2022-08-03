# Software Types

Schema.org profile for specifying software application types, used for software metadata descriptions.

**Authors**: Maarten van Gompel and Daniel Garijo

**Profile available at**: [https://w3id.org/software-types](https://w3id.org/software-types)

**Supported serializations**: JSON-LD (`application/ld+json`), Turtle (`text/turtle`) and HTML. See the code snippet below for an example on how to retrieve the profile in Turtle with a `curl` command:

```
curl -sH "accept:text/turtle" -L https://w3id.org/software-types
```

## Introduction

This profile describes vocabulary terms needed to describe the metadata of software application types (e.g., command line, desktop, software package, software library, etc.). The profile is meant to be used
with [schema.org](https://schema.org/) and [codemeta](https://codemeta.github.io). Our goal is to introduce
as little additional vocabulary as possible and only extend where schema.org
and codemeta leave representation gaps.

## Why do we need a schema.org profile for software types?

Software applications are ubiquitous in our society, ranging simple desktop applications in personal desktops to packages, libraries or containers deployed in servers. In fact, schema.org partly acknowledges the importance of software by having the term `schema:SoftwareApplication` as the target of `schema:SoftwareSourceCode`. This is because software applications typically offers one or more interfaces through which
users or machines can interact with it. Schema.org includes popular types of ``schema:SoftwareApplication``
like ``schema:WebApplication``, ``schema:MobileApplication``, and even
``schema:VideoGame``. However, we find that schema.org subtypes for software applications are quite lacking when we look at them from a software development perspective. Here we attempt to fill the gaps and define further (sub)types
such as command-line applications, desktops applications (GUIs) and programming libraries.

This availability of explicit software interface type information allows for
more accurate software metadata descriptions. Implementing these as types, in
line with what schema.org already does, allows for further type-specific
refinements. A good example is the
[WebAPI](https://github.com/schemaorg/schemaorg/issues/1423) type that is in
development which allows for describing Web APIs, though not a subtype of
``schema:SoftwareApplication``.

**Disclaimer**: this work aims to create a profile that may be incorporated into codemeta or schema.org. The profile has persistent identifiers, but, if standardized, the classes and properties defined here may be absorbed into other initiatives.

## Software types profile: Classes

We define the following additional vocabulary to describe software based on
interface type, most are a subclass of `schema:SoftwareApplication`.

* ``CommandLineApplication`` - A software application offering a command-line interface as the primary means of interaction. Examples are popular tools like: ``grep``, ``sed``, ``git``
* ``DesktopApplication`` - A software application offering a graphical user interface on the desktop. Examples are popular software like firefox, Microsoft Word, Facetime
* ``NotebookApplication`` - A web application in the form of a notebook (e.g. Jupyter Notebook, R Notebook) or data story.
* ``ServerApplication`` - A software application running as a daemon providing a service, either locally or over a network, running in the background. Examples are software like nginx, MySQL, postfix
* ``SoftwareImage`` - A software application in the form of an image (such as a container image or virtual machine image) that distributes the application along with its wider dependency context.
* ``SoftwareLibrary`` - A software application offering an Application Programming Interface (API) for developers. Examples are software like openssl, libxml2, blas, huggingface transformers, python-requests.
* ``SoftwarePackage`` - A software application in the form of a package for any particular package manager. It distributes the application but not necessarily its wider dependency context.
* ``TerminalApplication`` - A software application offering a terminal text-based user interface (a type of GUI constrained to the terminal, examples are popular tools like vim, mutt, htop, tmux, ncmpcpp, mc)


## Software types profile: Properties

### Executable name

The name of the executable within a certain run-time context (e.g. an
executable filename or name of an importable module). This documents on a
fairly high level by what name software is invoked from a certain run-time
context. The run-time context itself is turn loosely determined by properties
such as ``schema:runtimePlatform`` and ``schema:operatingSystem``.

We include this property to make a clear distinction between the human readable
name of the software, and the identifier used in invocation of the software.
The two may regularly differ with one being more verbose or have stricter
casing than the other.

Examples for this property are:

* The name of the invoked executable as invoked from the command line
* The name of the library as used in the linking stage for compiled languages
* The highest-level name of the module as invoked in an ``import`` or ``include`` statement in languages such as Python, R, Perl, Java.
* The name of the package as passed to a certain package manager (for ``SoftwarePackage``)
* The name of the container as known to a certain container registry (for ``SoftwareImage``)

Examples of this property are also shown in the code snippets A and B. 

Note that the executable name should typically not contain any
platform/runtime-specific extensions which may differ across systems
(``.exe``,``.so``,``.dll``,``.dylib``). However, such extensions may be
included if they are static over all possible systems and needed to invoke the
software (``.jar``,``.sh``) and a necessary component in invoking the software
from a specific context.

## How are software types terms used with codemeta?

Codemeta, building upon schema.org, describes software metadata focused on the
software's source code (`schema:SoftwareSourceCode`). We call the various
artefacts that can be produced by the source code **target products** and use
use the existing `schema:targetProduct` property to link the source code
description to one or more target products (see discussion
[here](https://github.com/codemeta/codemeta/issues/267)). These target products
take one of the types defines in our profile, or one of the existing ones
already in schema.org.

Example A (JSON-LD): An application named [WIDOCO](https://github.com/dgarijo/Widoco/) is both a command line application in Java (JAR), but also a library:

```json
{
    "@context": [
        "https://raw.githubusercontent.com/codemeta/codemeta/2.0/codemeta.jsonld",
        "https://raw.githubusercontent.com/schemaorg/schemaorg/main/data/releases/13.0/schemaorgcontext.jsonld",
        "https://w3id.org/software-types"
    ],
    "@type": "SoftwareSourceCode",
    "name": "WIDOCO",
    "version": "1.14.17",
    "codeRepository": "https://github.com/dgarijo/Widoco",
    ...,
    "targetProduct": [
        {
            "type": "CommandLineApplication",
            "name": "WIDOCO",
            "executableName": "Widoco-1.14.17-jar-with-dependencies.jar",
            "runtimePlatform": "Linux"
        },
        {
            "type": "SoftwareLibrary",
            "executableName": "es.oeg.Widoco",
            "name": "WIDOCO",
            "runtimePlatform": "Linux"
        },
    ]
}
```

Example B: A python package ([Chowlk](https://github.com/oeg-upm/Chowlk)) can be run as a web service, online (note that example links are provided):

```json
{
    "@context": [
        "https://raw.githubusercontent.com/codemeta/codemeta/2.0/codemeta.jsonld",
        "https://raw.githubusercontent.com/schemaorg/schemaorg/main/data/releases/13.0/schemaorgcontext.jsonld",
        "https://w3id.org/software-types"
    ],
    "@type": "SoftwareSourceCode",
    "name": "Chowlk",
    "codeRepository": "https://github.com/oeg-upm/Chowlk",
    ...,
    "targetProduct": [
        {
            "type": "WebApplication",
            "executableName": "chowlk-webapp",
            "provider": {
                "@type": "Organization",
                "name": "Ontology Engineering Group"
            }
            "url": "https://example.org/chwolk-ws"
        },
        {
            "type": "WebAPI",
            "provider": {
                "@type": "Organization",
                "name": "Ontology Engineering Group"
            }
            "endpointUrl": {
                "@type": "EntryPoint",
                "url": "https://example.org/chowlk-service",
                "contentType": "application/json"
            },
            "endpointDescription": {
                {
                  "@type": "CreativeWork",
                  "encodingFormat": "application/json",
                  "url": "https://example.org/chowlk-service/info?version=v1"
                },
            },
            "conformsTo": "https://jsonapi.org/format/1.0/",
            "documentation": "https://example.org/chowlk-service/docs",
        },
    ]
}
```

## Software as a Service 

The vocabulary we propose here allows to express a link between software
metadata and instances of that software running as a service somewhere. Example B shows ``schema:WebAPI`` and ``schema:WebApplication`` used to
describe instances of software as a service. We use ``schema:WebAPI`` with a
proposed extension as formulated
[here](https://webapi-discovery.github.io/rfcs/rfc0001.html) and discussed
[here](https://github.com/schemaorg/schemaorg/issues/1423). These are **not** new properties proposed in this profile.

The link between `SoftwareSourceCode` and subclasses of `SoftwareApplication`,
`WebAPI`, `WebPage` or `WebSite` is established using the `targetProduct`
property. There is also a reverse property for this called
``codemeta:hasSourceCode`` (see discussion
[here](https://github.com/codemeta/codemeta/pull/229)) that can be used in case of need.


## Implementations

Support for our `software_types` extension to codemeta/schema.org is
implemented in the following software:

* [codemetapy](https://github.com/proycon/codemetapy) - Python library and command-line tool for converting to codemeta and creating/manipulating existing codemeta descriptions.
* [codemeta-harvester](https://github.com/proycon/codemeta-harvester) - Automatically harvests and converts software metadata to codemeta
* [somef](https://github.com/KnowledgeCaptureAndDiscovery/somef) ([work in progress](https://github.com/KnowledgeCaptureAndDiscovery/somef/issues/486)) - A Python package for automated software metadata extraction. 
* [c2t](https://github.com/osoc-es/c2t) ([work in progress](https://github.com/osoc-es/c2t/issues/34)), a python package for converting software images into triples (extracting their dependencies)

## Related approaches

Many vocabularies exist to describe software or its constituent parts, e.g., the [software description ontology](https://w3id.org/okn/o/sd/), [description of a project vocabulary](http://usefulinc.com/ns/doap#), [hydra](https://www.hydra-cg.com/spec/latest/core/) (for API description), the common workflow language (description of inputs and outputs of software components, etc.), etc.  Our proposed profile does not aim to redefine any new term related to software, but propose a lightweight profile that can be easily incorporated into schema.org or codemeta.

## Acknowledgement

This work was indirectly and partially funded through the [CLARIAH-PLUS project](https://clariah.nl).

This work has been supported by the Madrid Government (Comunidad de Madrid-Spain) under the Multiannual Agreement with Universidad Politécnica de Madrid in the line Support for R&D projects for Beatriz Galindo researchers, in the context of the V PRICIT (Regional Programme of Research and Technological Innovation)
