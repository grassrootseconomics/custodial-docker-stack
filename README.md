## custodial-stack

Run the entire GE custodial stack with Docker on a local devnet with minimal
effort.

### Prerequisites

- [Docker](https://docs.docker.com/engine/install/)
- [Cast](https://book.getfoundry.sh/getting-started/installation)

### Dependency order

1. `services`.
2. `boostrap` (needs to be run only once).
3. `stack`.

### Setup

In the correct dependency order (run the docker compose commands in the
respecctive folders in the order given above):

1. Create a `custodial` overlay network with `docker network create custodial`.
2. Pull the images with `docker-compose pull`.
3. Bring up the containers with `docker compose up -d`.
4. Dump the current system signer key with
   `curl -X GET --header 'X-GE-KEY: xd' http://localhost:5003/api/v2/service/system`.
5. Add it as a writer to the custodial registry proxy smart contract and top it
   up with some gas:

```bash
cast send --async --gas-limit 1000000 --gas-price 5gwei --priority-gas-price 10wei --private-key 4bbbf85ce3377467afe5d46f804f221813b2bb87f24d81f60f1fcdbf7cbf4356 0xf282a3C68A2505a79Fc99f94CE43D9c83230CaE5 "setNewSystemAccount(address)" $SYSTEM_SIGNER_ADDRESS
cast send --async --gas-limit 1000000 --gas-price 5gwei --priority-gas-price 10wei --private-key 4bbbf85ce3377467afe5d46f804f221813b2bb87f24d81f60f1fcdbf7cbf4356 --value 50ether $SYSTEM_SIGNER_ADDRESS
```
