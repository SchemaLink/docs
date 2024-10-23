---
title: Developer guide
parent: Guides
layout: default
---

The SchemaLink project is made up of a number of components, all of which fall
under [the SchemaLink GitHub organization](https://github.com/SchemaLink). More
pragmatic information can also be found in the README files of each repository.

## The webapp

{: .important-title }

> In a glance
>
> - Repository: <https://github.com/SchemaLink/webapp>.
> - Main technologies: [Node](https://nodejs.org/docs/latest/api/),
>   [Vite](https://vite.dev/guide/),
>   [TypeScript](https://www.typescriptlang.org/docs/), JavaScript,
>   [React](https://react.dev/learn), [Redux](https://redux.js.org/usage/) and
>   [Semantic UI React](https://react.semantic-ui.com/).
> - Run locally - needs Node and npm:
>
>   ```shell
>   npm install
>   npx nx serve arrows-ts
>   ```

The webapp code was originally forked from Neo4J-labs's [Arrows
project](https://github.com/neo4j-labs/arrows.app). By now, it diverged from it
considerably, but it maintains the same structure. The repository is structured
as a monorepo, hosting both apps and libraries in the same codebase.

### Apps

SchemaLink uses only one app, stored in the [`apps/arrows-ts`
directory](https://github.com/SchemaLink/webapp/tree/main/apps/arrows-ts). The
`arrows-ts` app contains (mostly) TypeScript code that takes care of the visual
React components, as well as the Redux store. To do so, the app makes extensive
use of the libraries.

### Libraries

There are many libraries hosted in the webapp repository, in the [`libs`
directory](https://github.com/SchemaLink/webapp/tree/main/libs). Specifically:

- The `graphics` library provides the logic for rendering and interacting with
  the canvas.
- The `linkml` library takes care of anything related to LinkML, namely
  importing and exporting the graph from and to this language.
- The `model` library implements the data model for the main entities that are
  needed to run the webapp.
- The `ontology-search` library is an API which is supposed to function as an
  adapter to any ontology search engine, so that the webapp doesn't have to care
  about the specifics of such search engine. Right now the search engine that
  the library uses is <https://www.ebi.ac.uk/ols4/>.
- The `selectors` library gathers some Redux selectors that are generic enough
  to be useful for any app.

## The API

{: .important-title }

> In a glance
>
> - Repository: <https://github.com/SchemaLink/api>.
> - Main technologies: [Python](https://docs.python.org/3/) and
>   [FastAPI](https://fastapi.tiangolo.com/).
> - Run locally - needs Python:
>
>   ```shell
>   pip install -r requirements
>   fastapi dev main.py
>   ```

The API specification can be found at
<https://fl-14-97.zhdk.cloud.switch.ch/api/docs>, or
<https://fl-14-97.zhdk.cloud.switch.ch/api/redoc>. Endpoints in the `/linkml`
group use the [linkml python library](https://pypi.org/project/linkml/) to
assist the webapp in handling linkml schemas. Endpoints in the `/openai` group
use the [openai python sdk](https://pypi.org/project/openai/) to power the
GPT-based features of the webapp.

## The deployment

{: .important-title }

> In a glance
>
> - Repository: <https://github.com/SchemaLink/deploy>.
> - Main technologies:
>   [Ansible](https://docs.ansible.com/ansible/latest/index.html),
>   [Docker](https://docs.docker.com/),
>   [Traefik](https://doc.traefik.io/traefik/).

SchemaLink is deployed as a docker compose project. To achieve that, both the
webapp and the api offer a `Dockerfile`. On each tag on the main branch, the
docker image is built by a GitHub action and pushed to the GitHub container
registry. A traefik container is also part of the docker compose project, acting
as a reverse proxy and taking care of routing and securing the connection.

## The docs

{: .important-title }

> In a glance
>
> - Repository: <https://github.com/SchemaLink/docs>.
> - Main technologies: [Jekyll](https://jekyllrb.com/docs/),
>   [Markdown](https://www.markdownguide.org/), [Just the
>   Docs](https://just-the-docs.github.io/just-the-docs/).

The docs project is used to generate this website. To update this website, it's
enough that you commit and push your changes to the docs project. A GitHub
action will run on the latest commit on the main branch, building the website
using Jekyll and deploying it using GitHub pages.
