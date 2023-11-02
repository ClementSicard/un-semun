# 🇺🇳 SemUN repository

Repository for SemUN project. It is composed of a docker-compose stack, with:

- An API ([`un-semun-api`](un-semun-api))
- A frontend ([`un-semun-front`](un-semun-front))
- An NLP pipeline ([`un-ml-pipeline`](un-semun-db)).
- A Neo4j graph database (`neo4j` service in [`docker-compose.yml`](docker-compose.yml))
- Scripts to populate the database ([`un-semun-misc`](un-semun-misc))
- A scraper for the United Nations Digital Library

[![Built with OrbStack](https://img.shields.io/badge/built%20with-OrbStack-pink.svg)](https://orbstack.dev) [![Built with neo4j](https://img.shields.io/badge/built%20with-Neo4j-purple.svg)](https://neo4j.com) ![Packages](https://img.shields.io/badge/package%20manager-poetry-blue) ![Linter](https://img.shields.io/badge/built%20with-React-orange) ![Version](https://img.shields.io/github/v/release/ClementSicard/un-semun?display_name=tag&label=version&logo=python&logoColor=white)[![Built with HuggingFace](https://img.shields.io/badge/built%20with-Hugging%20Face%20🤗-cyan.svg)](https://huggingface.co) [![Built with spaCy](https://img.shields.io/badge/built%20with-spaCy-09a3d5.svg)](https://spacy.io)

## Table of Contents

- [🇺🇳 SemUN repository](#-semun-repository)
  - [Table of Contents](#table-of-contents)
    - [Description \& Paper](#description--paper)
    - [Running the project](#running-the-project)
      - [Install requirements](#install-requirements)
      - [Run the project](#run-the-project)
      - [Stop the stack](#stop-the-stack)
    - [Ingest documents using the ML pipeline API](#ingest-documents-using-the-ml-pipeline-api)

### Description & Paper

- To have more information on the project, please refer to the [project proposal](docs/project-proposal.pdf)
- For more details about the final result, please refer to the [paper](un-semun-paper/paper.pdf)

### Running the project

#### Install requirements

You also need to have Docker installed, I'm using [OrbStack](https://orbstack.dev/) as a Docker desktop client for macOS, but regular Docker installation works perfectly fine as well.

#### Run the project

When Docker is setup, you just have to run:

```bash
# Start the containers
docker-compose up -d
```

Open the frontend at [http://localhost:8080/](http://localhost:8080) if using Docker Desktop or [http://un-semun-frontend.un-semun.orb.local/](http://un-semun-frontend.un-semun.orb.local/) if using OrbStack.

#### Stop the stack

To stop the stack, just run:

```bash
docker-compose down
```

You are all set! 🎉

### Ingest documents using the ML pipeline API

To ingest documents, you can use the ML pipeline API. You can find more information about it in the [`README.md`](un-ml-pipeline/README.md) of the `un-ml-pipeline` folder.

You basically need to send a `POST` request to the `/run` endpoint at URL `http://un-semun-api.un-semun.orb.local` with a JSON body containing the following fields:

```json
[
    {"recordId": "<record_id_0>"},
    {"recordId": "<record_id_1>"},
    {"recordId": "<record_id_2>"},
    ...
]
```

You can also send a `POST` request to the `/run_search` endpoint, at the same URL, with a natural language query to the UN Digital Library. The API will then scrape the results and ingest them in the database.

```json
{
  "q": "<query>"
}
```

You can also include a limit number of results to scrape, by adding a field `"n": <value>` in the payload.

For instance:

```json
{
  "q": "Women in peacekeeping",
  "n": 256
}
```
