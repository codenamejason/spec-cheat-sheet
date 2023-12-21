# Spec Cheat Sheet
> Cheat sheet for spec.dev [reference](https://github.com/allo-protocol/allo-v2-spec/blob/main/README.md)

### Pre-requisites

- Active Spec account
- [Spec CLI](https://github.com/spec-dev/allo/blob/master/guides/CLI-Setup.md) installed (>= 0.1.25)
- Postgres (>= 14) and psql

Install Spec globally (this is not the CLI)
```bash
$ npm install -g @spec.dev/spec
```

### Initialize codebase as a spec projecr
```bash
$ spec init
```

### Linking to a spec project
```bash
$ spec link project namespace/projectName .
```

### Create a new Live Object
[Reference](https://github.com/spec-dev/allo/blob/master/guides/Writing-Live-Objects.md)

### Editing a Live Object
1. Edit the Live Object code.
2. Bump the version of the Live Object in your `mainfest.json` file.
3. Publish the Live Object to spec.
```bash
# The --open flag will open the Live Object in your browser after it's published.
spec publish LiveObject --open
```
4. Update the Live Object in your `project.toml` file.
5. Data will be migrated automatically.

### Add a contract group to spec ecosystem
```bash
$ spec add contract group
```

### Add a contract to a contract group
Typically, I would say use the spec add contracts command, but since I ended up swapping that out for spec sync contracts, the easiest way to "add" a contract to a factory group for now will be to just do something like:

1. Change your contract group inside `contracts.spec.json` to look something like this (i.e. give it the address you wanna add)

```javascript
{
    "name": "SQFSuperFluidStrategy",
    "isFactoryGroup": true,
    "contracts": [
    {
        "chainId": 5,
        "address": "0xabc123..."
    }
    ]
}
```
2. Then run the sync command
```bash
$ spec sync contracts
```
3. Now remove the `"contracts": [...]` key/value pair from that group inside contracts.spec.json (i.e. undo step 1)

### Adding a new migration
```bash
$ spec new migration the_name_of_the_migration
```

### Run migrations
```bash
$ spec migrate

# Run migrations in production with the --env flag
$ spec migrate --env prod
```

### Run spec locally
```bash
$ spec start
```

### Enable the GraphQL addon for your project
```bash
$ spec enable graphql
```

### Run a local instance of graphql on spec
```bash
$ spec run graphql
```

### Run tests
```bash
spec test LiveObject
```


[Reference](https://github.com/spec-dev/allo/blob/master/guides/Testing-Live-Objects.md)

### Tailing contract events
```bash
$ spec tail namespace.LiveObject.Event

# Every Live Object that's published to Spec will broadcast an event anytime one of its records is changed. These are the events your Live Tables subscribe to in order to stay up-to-date over time. Every Live Object event has the same naming structure, which is:
<namespace.LiveObject>Changed
```

### Deploying to spec
>🚨 Before you publish your Live Object:
> - Make sure you've run your migrations locally and that your local database is up-to-date.
> - Make sure you've run your tests locally and that they're passing.
> - Check local graphql endpoint to make sure it's working as expected.
> - Update the version of the Live Object in your `mainfest.json` & `project.toml` file.


```bash
$ spec publish LiveObject --open
```