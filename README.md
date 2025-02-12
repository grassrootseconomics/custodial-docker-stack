## custodial-stack

Run the entire GE custodial stack with Docker on a local devnet with minimal
effort.

### Prerequisites

- [Docker](https://docs.docker.com/engine/install/)
- [Cast](https://book.getfoundry.sh/getting-started/installation)

### Dependency order

1. `services`.
2. `stack`.

### Restore snapshots

```bash
wget https://dev-snapshots.grassecon.net/devnet.tar.bz2 https://dev-snapshots.grassecon.net/nats.tar.bz2 https://dev-snapshots.grassecon.net/postgres.tar.bz2
docker run -i -v services_custodial-nats-data:/volume --rm loomchild/volume-backup restore < nats.tar.bz2
docker run -i -v services_custodial-postgres-data:/volume --rm loomchild/volume-backup restore < postgres.tar.bz2
docker run -i -v services_custodial-reth-data:/volume --rm loomchild/volume-backup restore < devnet.tar.bz2
```

### Setup

In the correct dependency order (run the docker compose commands in the
respecctive folders in the order given above):

1. Create a `custodial` overlay network with `docker network create custodial`.
2. Pull the images with `docker-compose pull`.
3. Bring up the containers with `docker compose up -d`.
4. Dump the current system signer key
5. Add it as a writer to the custodial registry proxy smart contract and top it
   up with some gas:

### Snapshot details

```
DEPLOYER_PUBLIC_KEY=0xF7052A82bb04CB3510D9D6E27eB9Ef4359549e3A
DEPLOYER_PRIVATE_KEY=0x18461f06582ad99b221cdf3ee5a1ea813aa6a88f7001d8d6c5d9de7c32690657
TOKEN_INDEX=0x7e9143817cac4C68557A2526781E8bD96C6ce5A1
USER_INDEX=0x777df3C3BcC9f30b4073e913610fCd2FeCad267C
GAS_FAUCET=0x0dF9f41D5CBc70eA348E98A0D69F7a970e5D3193
PERIOD_BACKEND=0xb2c39a093fCB324Bd2F1A0D201BAF138682401E6
CUSTODIAL_REGISTRATION_PROXY=0x69D6FD9CC92f591352Ec2d9ffd9094AA3D4DF304
POOL_INDEX=0x225e641D4c163f7E18F413DD7E6B0178d2Bc783e
REGISTRY=0xEA7a52e565C43598011cC5f509b8252Eb3e8dbE5
# Tokens
KILIFI=0xeA03669e7D2d8e419aaffaD86939321DF599Cb03
NRB=0x31F0d84fBC29c15b5EbE2d717f4586E933Cad3fC
# Pools
LIMITER=0x65AE896585d1dcc43e16d94235aCB182dBDe5C47
CITY_POOL=0x3b517308D858a47458aD5C8E699697C5dc91Da0F
QUOTER=0xCFF402fFa03Cb7c0f957A6fAE8067c334460ee64
# SYSTEM_SIGNER_PUBLIC_KEY=0x8CA0b7f9458fF04Eb214248f3cDA6d0a5D2f88D3
```
