# Ansible Role: geth

### Description

Ansible role that will install, configure and runs Geth

### Table of Contents

- [Supported Platforms](#supported-platforms)
- [Dependencies](#dependencies)
- [Role Variables](#role-variables)
- [Example Playbook](#example-playbook)
- [License](#license)
- [Author Information](#author-information)

### Supported Platforms

```
* Debian
* Ubuntu
* Redhat(CentOS/Fedora)
* Amazon
```

### Dependencies

- Go 1.13.x or greater

### Role Variables:

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file. By and large these variables are configuration options.

| Name                         | Default Value                                                                                             | Description                                                                                                                                                                               |
|------------------------------|-----------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `geth_version`               | **_unset_**                                                                                               | Version of Geth to install and run. All available versions are listed on our Geth [releases](https://geth.ethereum.org/downloads/) page                                                   |
| `geth_git_hash`              | **_unset_**                                                                                               | Commit hash of Geth to install and run. Must match geth_version. All available versions are listed on our Geth [releases](https://geth.ethereum.org/downloads/) page                      |
| `geth_user`                  | geth                                                                                                      | Geth user                                                                                                                                                                                 |
| `geth_group`                 | geth                                                                                                      | Geth group                                                                                                                                                                                |
| `geth_download_url`          | https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-{{geth_version}}-{{geth_git_hash}}.tar.gz | The download tar.gz file used. You can use this if you need to retrieve geth from a custom location such as an internal repository.                                                       |
| `geth_install_dir`           | /opt/geth                                                                                                 | Path to install to                                                                                                                                                                        |
| `geth_config_dir`            | /etc/geth                                                                                                 | Path for default configuration                                                                                                                                                            |
| `geth_node_private_key_file` | ""                                                                                                        | Path for node private key, if supplied. This needs to include the node key file name and path like so `/home/me/me_node/myPrivateKey`. If not supplied Geth will create one automatically |
| `geth_data_dir`              | /opt/geth/data                                                                                            | Path for data directory                                                                                                                                                                   |
| `geth_log_dir`               | /var/log/geth                                                                                             | Path for logs                                                                                                                                                                             |
| `geth_managed_service`       | true                                                                                                      | Enables a systemd service                                                                                                                                                                 |
| `geth_systemd_dir`           | /etc/systemd/system/                                                                                      | The default systemd directory                                                                                                                                                             |
| `geth_systemd_state`         | restarted                                                                                                 | The default option for the systemd service state                                                                                                                                          |
| `geth_identity`         | GethNode                                                                                                  | Identity of the node                                                                                                                                                                      |
| `geth_host_ip`               | ""                                                                                                        | The host IP that Geth uses for the P2P network. This specifies the host on which P2P listens                                                                                              |
| `geth_discovery_public_ip`   | true                                                                                                     | Spefies whether the node should use the public IP of the host in cloud (AWS,Azure,GCP). In private networks, the private IP is more secure and faster for traffic to route                |
| `geth_network_id`            | 1337                                                                                                      | The id of the network, also specified in the genesis file                                                                                                                                 |
| `geth_sync_mode`             | snap                                                                                                      | Specifies the synchronization mode. Other values are 'fast'                                                                                                                               |
| `geth_log_verbosity`         | 3                                                                                                         | The log level to use. Other log levels are 0=silent, 1=error, 2=warn, 3=info, 4=debug, 5=detail                                                                                           |
| `geth_metrics_enabled`       | true                                                                                                      | Enable collection of prometheus metrics                                                                                                                                                   |
| `geth_metrics_host`          | 0.0.0.0                                                                                                   | pprof HTTP server listening interface                                                                                                                                                     |
| `geth_metrics_port`          | 9545                                                                                                      | pprof HTTP server listening port                                                                                                                                                          |
| `geth_p2p_port`              | 30303                                                                                                     | Specifies the P2P listening ports (UDP and TCP). Ports must be exposed appropriately                                                                                                      |
| `geth_http_enabled`          | true                                                                                                      | Enabled the HTTP JSON-RPC service                                                                                                                                                         |
| `geth_http_host`             | 127.0.0.1                                                                                                 | Specifies the host on which HTTP JSON-RPC listens                                                                                                                                         |
| `geth_http_port`             | 8545                                                                                                      | Specifies the port on which HTTP JSON-RPC listens                                                                                                                                         |
| `geth_http_api`              | ["admin","db","eth","debug","miner","net","shh","txpool","personal","web3"]                               | Comma-separated APIs to enable on the HTTP JSON-RPC channel. When you use this option, the `geth_rpc_http_enabled` option must also be enabled                                            |
| `geth_http_cors_origins`     | ["all"]                                                                                                   | Comma separated list of domains from which to accept cross origin requests                                                                                                                |
| `geth_http_virtual_hosts`    | ["all"]                                                                                                   | Comma separated list of virtual hostnames from which to accept requests                                                                                                                   |
| `geth_ws_enabled`            | true                                                                                                      | Enabled the WebSockets service                                                                                                                                                            |
| `geth_ws_api`                | ["admin","db","eth","debug","miner","net","shh","txpool","personal","web3"]                               | Comma-separated APIs to enable on the HTTP JSON-RPC channel. When you use this option, the `geth_rpc_ws_enabled` option must also be enabled                                              |
| `geth_ws_host`               | 0.0.0.0                                                                                                   | Specifies the host on which WebSockets listens                                                                                                                                            |
| `geth_ws_port`               | 8546                                                                                                      | Specifies Websockets JSON-RPC listening port (TCP). Port must be exposed appropriately                                                                                                    |
| `geth_ws_origins`            | ["all"]                                                                                                   | Comma separated list of domains from which to accept websockets requests                                                                                                                  |
accept requests                                                                                                                   |
| `geth_user_cmdline_args`     | ""                                                                                                        | Command line args that are passed in from the user                                                                                                                                        |
| `geth_env_opts`              | []                                                                                                        | Settings passed to the Geth through environment variables                                                                                                                                 |
| `geth_unlock`                | 0                                                                                                         | Comma separated list of accounts to unlock                                                                                                                                                |
| `geth_account_password_file` | ""                                                                                                        | Password file to use for non-interactive password input                                                                                                                                   |


### License

Apache

### Author Information

Consensys, 2022
