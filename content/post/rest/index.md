+++
title = "REST"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
REST (Representational State Transfer) is a set of principles introduced in 1999 that suggest a way of designing and distributing software.

Principles of REST:

- Everything is a resource
- Each resource is accessible via a unique URI
- Resources can have multiple representations
- Communication is done over the stateless protocol (HTTP)
- Management of resources is done via HTTP methods

REST APIs, have six constraints: 

1. **client-server** architecture.
2. **stateless** architecture: each request should stand on its own, and order should not matter. No shared state.
3. **cacheable**: improves network performance.
    - `GET`, `PUT`, and `DELETE` should be *idempotent* (the same command executed multiple times, the state of resources on the server is exactly the same, much like pure functions)
    - `POST` is *not* idempotent.
    - Caching is a way to store and retrieve data so that future requests can be fulfilled faster without repeating expensive calculations or operations.
4. **layered system**: component A (a client) might or might not communicate directly with component B (the server). There may be other layers between them like logging, caching, DNS servers, load balancers, and authentication.
5. **code on demand**
    - The API returns the resource and code to act on it.
    - The client only needs to know how to execute the code.
    - Makes the API more flexible, upgradable and extendible.
    - Most web application, send JavaScript code along with the data.
6. **uniform interfaces**
    - Each resource should be accesible through a single url. Not a hard requirement, but recommended.
    - We should be able to manage the resources through these *representations* (the URL).
    - every interaction with the resource should happen through the URL identifier we gave to it.
    - Self-descriptive messages.
    - **HATEOAS** (**H**yepermedia **A**s **T**he **E**ngine **O**f **A**pplication **S**tate). Much like a *choose your own adventure book*, the pages are not read in order. You start at page 1, and based on the information available, the reader (client) chooses the action to take, moving them to a different page.

[Representational State Transfer (REST)](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)