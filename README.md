# N.B. Rebom Project is Moved under [ReARM](https://github.com/relizaio/rearm)

This repository will be preserved for the purposes of tutorials built on top of that but is not maintained since 2025-05-20.

## Rebom by Reliza

## Catalog of Software Bills of Materials (SBOMs)

Rebom works with [CycloneDX](https://cyclonedx.org) JSON standard.

Public demo is available at https://rebomdemo.relizahub.com .

*N.B.* (!) Please note, that any boms uploaded to demo are publicly visible (!)

Rebom is also a storage part of our larger product [ReARM](https://github.com/relizaio/rearm).


## Load SBOM to Rebom
Download [Reliza CLI](https://github.com/relizaio/reliza-cli) for your platform.

Add reliza-cli to path and run following command to load bom json:

```
reliza-cli rebom put --rebomuri "https://rebomdemo.relizahub.com" --infile "tests/bom_versioning_1.json"
```

Flags used:
- --rebomuri - set to the URI where rebom server is deployed
- --infile - set to absolute or relative path of the bom file to be added. To test, you may use samples from the tests directory of this repository.

Note that only CycloneDX BOMs in JSON are supported.


## Running Rebom packages

### With Docker-compose
Clone this repository and run the following command:

```
docker-compose up -d
```

### With Kubernetes and Helm

You would need Helm pre-installed. 

#### Option 1. Install from Reliza Registry:

```
helm upgrade --install --create-namespace rebom -n rebom oci://registry.relizahub.com/library/rebom
```

#### Option 2. Install from local clone of this repository:

Clone this repository. Then run:

```
helm upgrade --install --create-namespace rebom -n rebom helm/rebom
```

## References
This project is mentioned in the following tutorials and external resources:

1. [How To Spin Helm Ephemerals with Reliza Hub: Tutorial](https://worklifenotes.com/2023/04/19/how-to-spin-helm-ephemerals-with-reliza-hub-tutorial/)
2. [CycloneDX Tool Center](https://cyclonedx.org/tool-center/)

## Developing Rebom

You would need Docker and Node 16+. Visual Studio Code is recommended.

1. Create a docker container for database:
```
docker run --name rebom-postgres -d -p 5438:5432 -e POSTGRES_PASSWORD=password postgres:14
```

You can later start/stop database container using `docker stop rebom-postgres` and `docker start rebom-postgres`.

2. Run Flyway - you can do it using docker:
 
```
docker run --rm -v $PWD/backend/migrations:/flyway/sql flyway/flyway -url=jdbc:postgresql://host.docker.internal:5438/postgres -user=postgres -password=password -defaultSchema=rebom -schemas='rebom' migrate
```

3. Run backend:

Backend is an Express.js / Apollo GraphQL project.

```
cd backend
npm install
npm start
```

Backend will be listening on localhost, port 4000.

#### Configure OCI Repo
Set the following ENV to enable oci repository

```
export OCI_STORAGE_ENABLED=true
export OCI_ARTIFACT_SERVICE_HOST=http://localhost:8083
export OCIARTIFACTS_REGISTRY_HOST=<registry_host_without_trailing_slash>
export OCIARTIFACTS_REGISTRY_NAMESPACE=<registry_namespace_without_trailing_slash>
```

4. Run frontend:

Frontend is a Vue 3 project.

```
cd frontend
npm install
npm run serve
```

Frontend will be listening on localhost, port 3005.

Navigating to http://localhost:3005 in your browser should open Rebom.


## Support

Please ask questions or share comments at DevOps and DataOps Discord - https://devopscommunity.org - for support.
