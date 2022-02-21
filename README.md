# software_types
Schema.org profile for software types

Authors: Maarten van Gompel and Daniel Garijo

Profile available at: https://w3id.org/software-types

## Why do we need a profile for software types?

A software application typically offers one or more interfaces through which users or machines can interact with it.
This information is already partially available in schema.org as there are subtypes of ``schema::SoftwareApplication`` like ``schema:WebApplication``, ``schema:MobileApplication``, and even ``schema:VideoGame``. The ``software_types`` profile developed here attempts to
fill the gaps and define further (sub)types such as command-line applications, desktops applications (GUIs) and programming libraries.

This availability of explicit software interface type information allows for more accurate software metadata
descriptions. Implementing these as types, in line with what schema.org already does, allows for further type-specific
refinements. A good example is the [WebAPI](https://github.com/schemaorg/schemaorg/issues/1423) type that is in development which allows for descripting Web APIs, though not a subtype of ``schema:SoftwareApplication``.

## Examples

Why would we want to have these additional types? We should motivate each of them here.

## Mapping existing work

Explain how we map other work like cwl, hydra, doap, etc. Many works exist for similar purposes!
