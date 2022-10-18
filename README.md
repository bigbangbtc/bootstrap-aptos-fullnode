## Run aptos fullnode with docker-compose


### file description:

- these files are copied from https://github.com/aptos-labs/aptos-networks/tree/main/mainnet with no changes.

```
├── genesis.blob
├── waypoint.txt
```


- these files are copied from https://github.com/aptos-labs/aptos-core/tree/main/docker/compose/aptos-node with minimum changes.

```
├── blocked.ips
├── docker-compose-fullnode.yaml
├── fullnode.yaml
├── haproxy-fullnode.cfg
```


### Steps to bootstrap your full node:


1. ~~generate keys: aptos genesis generate-keys --output-dir keys~~

2. bootstrap
```
$ docker-compose -f docker-compose-fullnode.yaml up -d
```

3. Verify initial synchronization

    During the initial synchronization of your fullnode, there may be a lot of data to transfer. You can monitor the progress by querying the metrics port to see what version your node is currently synced to. Run the following command (or using `./scripts/check_synced_version.sh`) to see the current synced version of your node:

    ```
    curl 127.0.0.1:9101/metrics 2> /dev/null | grep "aptos_state_sync_version{.*\"synced\"}" | awk '{print $2}'
    ```

    The command will output the current synced version of your node. For example: `71000`

    Compare the synced version returned by this command (e.g., 71000) with the Current Version (latest) shown on the Aptos status page. If your node is catching up to the current version, it is synchronizing correctly.