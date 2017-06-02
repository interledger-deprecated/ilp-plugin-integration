# ILP Plugin Integration
> Test a ledger plugin against two live ILP Kit instances

## Setup

### Build

Requires [Docker](https://www.docker.com/) and [Docker Compose](https://docs.docker.com/compose/).
To pull all the required images in, run:

```sh
$ docker-compose build
```

### Environment Variables

- `ILP_PLUGIN_MODULE` - The module that contains your plugin. This must also be a directory inside the current directory, so that it can be sent to the docker daemon and used as a volume.
- `ILP_PLUGIN_PREFIX` - The ledger prefix that your plugin uses. It should match the value of `plugin.getInfo().prefix`. This is required in order to automatically configure a connector.
- `ILP_PLUGIN_STORE` - (Optional) If your plugin makes use of the [Plugin Store](https://github.com/interledger/rfcs/blob/master/0004-ledger-plugin-interface/0004-ledger-plugin-interface.md#_store), this will tell the connector to pass one into the constructor. Must be `true` or `false` if specified.

- `ILP_PLUGIN_CONFIG_1` - A JSON string specifying the constructor parameters to your first plugin, eg. `{"account":"https://red.ilpdemo.org/ledger/accounts/test","password":"test"}`.
- `ILP_PLUGIN_ACCOUNT_1` - The full ILP address of your first plugin. It should match the value of `plugin.getAccount()`. This is required in order to automatically configure a connector.

- `ILP_PLUGIN_CONFIG_2` - As `ILP_PLUGIN_CONFIG_1`, but specifying a second plugin.
- `ILP_PLUGIN_ACCOUNT_2` - As `ILP_PLUGIN_ACCOUNT_1`, but with the ILP address of your second plugin.

### Running

Make sure that all the environment variables in the above session are in the environment.
It is useful to write a shell script to do this. To set up your two ILP Kits and their connectors,
run:

```sh
$ docker-compose up
```

### Test the Environment

If you want to access the ILP Kits on your machine, you'll need to add the following lines to your `/etc/hosts` file:

```
127.0.0.1 ilp-kit1
127.0.0.1 ilp-kit2
```

Once you start the docker-compose, you can access `http://ilp-kit1:3010` or `http://ilp-ki2:4010` on your machine.
Log in with the username `admin`, and the password `password`. You should be able to send from `ilp-kit1` to `admin@ilp-kit2:4010`,
or from `ilp-kit2` to `admin@ilp-kit1:3010`.
