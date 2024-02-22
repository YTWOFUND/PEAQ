### Peaq

[Official documentation](https://docs.peaq.network/docs/node-operators/become-collator/) </br>

### System requirements: </br>

### Minimum: </br>
OS - Ubuntu 20.04
CPU - 3.3 GHz AMD EPYC 7002
Storage - 500GB NVMe SSD
Memory - 8GB

# Installation of the Taiko node with Docker.
### 1. Required packages installation:
```
sudo apt update && apt upgrade -y
```
```
sudo apt install git-all -y
```
### 2. Installing docker and docker compose:
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
curl -SL https://github.com/docker/compose/releases/download/v2.16.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
### 3. Set up a Node
```

sudo docker run -d -v krest-storage:/chain-data -p 9944:9944 -p 9933:9933 peaq/parachain:krest-v0.0.5 \
--collator \
--parachain-id 2241 \
--chain ./node/src/chain-specs/krest-raw.json \
--base-path chain-data \
--port 30333 \
--rpc-port 9944 \
--rpc-methods=unsafe \
--unsafe-rpc-external \
--rpc-cors=all \
--execution=wasm \
-- \
--execution wasm \
--chain ./node/src/chain-specs/kusama.json \
--port 30343 \
--sync warp \
--rpc-port 9977
```
### 4. Generate a Session Key
```
curl -X 'POST' \
-H "Content-Type: application/json" \
-d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9944

Copy the key from the response:
{"jsonrpc":"2.0","result":"0xf22d82cfcf4402990a0bef3abefd1e58217ff3a86c548d30f1495fd529460d76","id":1}

In this case, the key is:
0xf22d82cfcf4402990a0bef3abefd1e58217ff3a86c548d30f1495fd529460d76
```

### Useful commands:

### Stop a node:
```
docker compose down
```
#### Remove a node:
```
docker compose down -v
```
#### Update a node:
```
docker compose pull
```
#### View the node's logs:
```
docker compose logs -f
```
