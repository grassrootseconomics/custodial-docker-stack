## custodial-stack

Run the entire GE custodial stack with Docker on a local devnet with minimal
effort.

### Prerequisites

- [Docker](https://docs.docker.com/engine/install/)
- [Cast](https://book.getfoundry.sh/getting-started/installation)

### Dependency order

1. `services`.
2. `stack`.

### Setup

In the correct dependency order (run the docker compose commands in the
respecctive folders in the order given above):

1. Create a `custodial` overlay network with `docker network create custodial`.
2. Pull the images with `docker-compose pull`.
3. Bring up the containers with `docker compose up -d`.
4. Dump the current system signer key
5. Add it as a writer to the custodial registry proxy smart contract and top it
   up with some gas:
