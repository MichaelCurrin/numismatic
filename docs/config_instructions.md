# Configuration Instructions

Set values for assets, currencies and API keys for `coin` to read.

These are the possible approaches:

1. Command-line arguments
2. Environment variables
3. Config file
4. System defaults

A value can be configured at all levels, however an item set higher up will override any items below it.


### 1. Command-line arguments

Set variables in the command-line when they are needed, without persisting them.

```bash
# Set assets and currencies.
coin prices --assets ATL --currencies ZAR
# Set assets and use configured or default currencies.
coin info -a BTC,ETH
coin info -a BTC -a ETH

# Set exchange and currencies and use configured or default assets.
coin listen --exchange luno --api-key-id 123456abc \
  --api-key-secret abcdef123 --currencies USD,ZAR collect run
```


### 2. Environment variables

Use the `export` command to set variables in your current shell session, so `coin` can access them when it runs in that same session. Note that the values will be lost when that shell session is closed.

```bash
# Set assets and currencies as environment variables.
export NUMISMATIC_ASSETS='ATL'
export NUMISMATIC_CURRENCIES='ZAR'
coin prices

export NUMISMATIC_ASSETS='BTC,ETH'
# Unset the currencies value which was set previously.
coin info

# Set exchange and currencies and restore assets. Setting of API keys with
# export is not currently supported.
export NUMISMATIC_EXCHANGE='luno'
export NUMISMATIC_CURRENCIES='USD,ZAR'
export -n NUMISMATIC_ASSETS
coin listen collect run
```

### 3. Config file

Create a persistent coin config file in your home directory. Use it to store your preferred assets and currencies settings for each exchange, as well as any API keys.

Update the config file as you need to. The values can also be overridden anytime by applying the arguments or environment variable approaches as covered above.

1. Create an empty text file.

    ```bash
    cd ~
    touch .coinrc
    ```

2. Open it in a text editor and fill it with desired values in the following format. Values can be disabled by commenting them out or leaving them as empty strings. Note that these environment values are _only_ set in the context of a specific exchange name on the prices feature.

    ```
    # Numismatic configuration file.

    [Bitfinex]
    assets: ATL
    currencies: USD,ZAR

    [Luno]
    assets:
    currencies: ZAR
    api_key_id: 123456abc
    api_key_secret: abcdef123
    ```


### 4. System defaults

If none of the previous approaches are used set a variable, `coin` will fall back to the hard-coded system default, if there is one. These are usually set as global constants in [cli.py](numismatic/cli.py).
