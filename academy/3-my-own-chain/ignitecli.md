---
title: "Ignite CLI"
order: 4
description: An easy way to build your application-specific blockchain
tag: deep-dive
---

# Ignite CLI

<HighlightBox type="prerequisite">

Before diving into the details of how Ignite CLI helps you scaffold the basics for your application blockchain make sure to understand the main concepts presented in the following sections:

* [A Blockchain App Architecture](../2-main-concepts/architecture.md)
* [Accounts](../2-main-concepts/accounts.md)
* [Transactions](../2-main-concepts/transactions.md)
* [Messages](../2-main-concepts/messages.md)
* [Modules](../2-main-concepts/modules.md)
* [Protobuf](../2-main-concepts/protobuf.md)
* [BaseApp](../2-main-concepts/base-app.md)

</HighlightBox>

<HighlightBox type="learning">

In this section, you will:

* Install the Ignite CLI.
* Scaffold a blockchain.
* Use the CLI.
* Start the Ignite UI server.
* Send your first message.

You can follow a hands-on exercise for Ignite CLI in the sections that follow this introduction.

</HighlightBox>

The Cosmos SDK provides the building blocks for a complete Tendermint blockchain, which implements the Inter-Blockchain Communication Protocol (IBC). The _BaseApp_ of the Cosmos SDK assembles these building blocks and provides a fully-running blockchain. All there is left to do for the specific blockchain application is to create specific modules and integrate them with BaseApp to make the application _your own_.

<HighlightBox type="info">

Ignite CLI assists with scaffolding modules and integrating them with BaseApp. Ignite CLI is a command-line tool that writes code files and updates them when instructed to do so. If you come from an _on Rails_ world, the concept will look familiar to you.
<br></br>
Ignite CLI also handles some compilation, runs a local blockchain node, and helps you with other tasks.

</HighlightBox>

<YoutubePlayer videoId="MTUQQ6nOkZo"/>

## Install

<HighlightBox type="reading">

Want to dedicate some time to dive deeper into installing Ignite CLI? Learn [how to install Ignite CLI in the Ignite CLI Developer Guide](https://docs.ignite.com/guide/install.html).

</HighlightBox>

This entire exercise was built using the Ignite CLI version 0.17.3. To install it at the command line:

```sh
$ curl https://get.ignite.com/cli@v0.17.3! | bash
```

Or if you install it in a Linux VM:

```sh
$ curl https://get.ignite.com/cli@v0.17.3! | sudo bash
```

You can verify the version of Ignite CLI you have once it is installed:

```sh
$ ignite version
```

This prints its old name and its version:

```
starport version v0.17.3
...
```

<HighlightBox type="info">

This entire exercise was built using the Ignite CLI version noted above. Using a newer version could work, but you might run into compatibility issues if you clone any code made with _this_ version of Ignite CLI and then try to continue the project with _your_ version of Ignite CLI.
<br></br>
If you need to install the latest version of Ignite CLI, use:

```sh
$ curl https://get.ignite.com/cli@! | bash
```

When you then run `ignite version`, it prints its new name and its version:

```
Ignite CLI version: v0.20.4
```

</HighlightBox>

<HighlightBox type="docs">

If you'd like to upgrade an existing project to the latest version of Ignite CLI, you can follow the [Ignite CLI migration documentation](https://docs.ignite.com/migration/).

</HighlightBox>

You can also just type `ignite` to see the offered commands:

```
Ignite CLI is a tool for creating sovereign blockchains built with Cosmos SDK, the world’s
most popular modular blockchain framework. Ignite CLI offers everything you need to scaffold,
test, build, and launch your blockchain.

To get started create a blockchain:

ignite scaffold chain github.com/cosmonaut/mars

Usage:
  ignite [command]

Available Commands:
  scaffold    Scaffold a new blockchain, module, message, query, and more
  chain       Build, initialize and start a blockchain node or perform other actions on the blockchain
  generate    Generate clients, API docs from source code
  network     Launch a blockchain network in production
  relayer     Connects blockchains via IBC protocol
  tools       Tools for advanced users
  docs        Show Ignite CLI docs
  version     Print the current build information
  help        Help about any command

Flags:
  -h, --help   help for ignite

Use "ignite [command] --help" for more information about a command.
```

## Your chain

Start by scaffolding a basic chain called `checkers` that you will place under the GitHub path `alice`:

```sh
$ ignite scaffold chain github.com/alice/checkers
```

<ExpansionPanel title="Troubleshooting">

For the sake of good support, the versions of all software used are communicated as encountered throughout the chapters and sections. It is natural that after the writing of the course material some version changes will appear, and it may occur that something breaks. Instead of using different versions of the software from the ones in the course, please look at the following list, which might fix problems you are running into.

<p>&nbsp;</p>

<PanelListItem number="1">

**Apple M1**

If you work with a machine using M1 architecture, make sure:

1. You follow this course in a [Rosetta](https://www.courier.com/blog/tips-and-tricks-to-setup-your-apple-m1-for-development/) terminal. 
2. You install [Homebrew](https://brew.sh/index).
3. You install Golang with `brew install go`.

</PanelListItem>

<PanelListItem number="2">

**Building Errors during `scaffold`**

If you work with Go 1.18, you may need to install the following:

* go install github.com/grpc-ecosystem/grpc-gateway/protoc-gen-grpc-gateway@latest
* go install github.com/grpc-ecosystem/grpc-gateway/protoc-gen-swagger@latest
* go install github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-openapiv2@latest
* git clone https://github.com/regen-network/cosmos-proto
  cd cosmos-proto/protoc-gen-gocosmos
  go install
* go get github.com/golangci/golangci-lint/cmd/golangci-lint
* go get golang.org/x/crypto/ssh/terminal@v0.0.0-20220411220226-7b82a4e95df4

</PanelListItem>

</ExpansionPanel>

The scaffolding takes some time as it generates the source code for a fully functional, ready-to-use blockchain. Ignite CLI creates a folder named `checkers` and scaffolds the chain inside it.

The `checkers` folder contains several generated files and directories that make up the structure of a Cosmos SDK blockchain. It contains the following folders:

* `app`: a folder for the application.
* `cmd`: a folder for the command-line interface commands.
* `proto`: a folder for the Protobuf objects definitions.
* `vue`: a folder for the UI.
* `x`: a folder for all your modules, in particular `checkers`.

<HighlightBox type="docs">

If Vue.js is something new to you, check out the [Vue.js website](https://vuejs.org/) for more on this JavaScript framework.

</HighlightBox>

If you look at the code that Ignite CLI generates, for instance in `./x/checkers/module.go`, you will often see comments like the following:

```go
// this line is used by starport scaffolding # 1
```

<HighlightBox type="warn">

Do not remove or replace any lines like these in your code as they provide markers for Ignite CLI on where to add further code when instructed to do so. For the same reason, do not rename or move any file that contains such a line.

</HighlightBox>

Go to the `checkers` folder and run:

```sh
$ cd checkers
$ ignite chain serve
```

The `ignite chain serve` command downloads dependencies and compiles the source code into a binary called `checkersd`. The command:

* Installs all dependencies.
* Builds Protobuf files.
* Compiles the application.
* Initializes the node with a single validator.
* Adds accounts.

After this command completes, you have a local testnet with a running node. What about the added accounts? Take a look:

<CodeGroup>

<CodeGroupItem title="config.yml" active>

```yaml
accounts:
  - name: alice
    coins: ["20000token", "200000000stake"]
  - name: bob
    coins: ["10000token", "100000000stake"]
validator:
  name: alice
  staked: "100000000stake"
client:
  vuex:
    path: "vue/src/store"
  openapi:
    path: "docs/static/openapi.yml"
faucet:
  name: bob
  coins: ["5token", "100000stake"]
```

</CodeGroupItem>

</CodeGroup>

In this file you can set the accounts, the accounts' starting balances, and the validator. You can also let Ignite CLI generate a client and a faucet. The faucet gives away five `token` and 100,000 `stake` tokens belonging to Bob each time it is called.

You can observe the endpoints of the blockchain in the output of the `ignite chain serve` command:

```
🌍 Tendermint node: http://0.0.0.0:26657
🌍 Blockchain API: http://0.0.0.0:1317
🌍 Token faucet: http://0.0.0.0:4500
```

<HighlightBox type="info">

Ignite CLI can detect any change to the source code. When it does, it immediately rebuilds the binaries before restarting the blockchain and keeping the state.

</HighlightBox>

## Interact via the CLI

You can already interact with your running chain. With the chain running in its shell, open another shell and try:

```sh
$ checkersd status
```

This prints:

```json
{"NodeInfo":{"protocol_version":{"p2p":"8","block":"11","app":"0"},"id":"63dc1053229905d7adba6a5161f0c212e6f5acab","listen_addr":"tcp://0.0.0.0:26656","network":"checkers","version":"0.34.11","channels":"40202122233038606100","moniker":"mynode","other":{"tx_index":"on","rpc_address":"tcp://0.0.0.0:26657"}},"SyncInfo":{"latest_block_hash":"77C7804FF75AE79BAE557F5AA1F00ABC399D7624F0F3A2B94AB168E3E8BD4006","latest_app_hash":"73C4B8BF40C133CF216E7EB25069A316C41262D092306EEE660FB952FF56EB2D","latest_block_height":"142","latest_block_time":"2021-12-20T15:09:04.028913Z","earliest_block_hash":"34AA6A6587F45F44FFDA4F77B0D3DFE1ADE53CE507728AC7AB5D040E1721F298","earliest_app_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855","earliest_block_height":"1","earliest_block_time":"2021-12-20T15:06:27.26818Z","catching_up":false},"ValidatorInfo":{"Address":"D8B48F2AE4C6A9F85821461D3CF0E1AB5311E008","PubKey":{"type":"tendermint/PubKeyEd25519","value":"nqOmhvFz6p1iMU6CSZNx60jDrAoYS3sJ6Bxkvc6JDRQ="},"VotingPower":"100"}}
```

In there you can see a hint of liveness: `"latest_block_height":"142"`. You can use this one-liner for better display:

```sh
$ checkersd status 2>&1 | jq
```

You can learn a lot by going through the possibilities with:

```sh
$ checkersd --help
$ checkersd status --help
$ checkersd query --help
```

And so on.

## Your GUI

Boot up the frontend created by Ignite CLI by using the commands provided in the `readme.md` file of the `checkers` folder. Let the chain run in its own process and open a new terminal window in your `checkers` folder. In this terminal execute:

```sh
$ cd vue
$ npm install
$ npm run serve
```

<ExpansionPanel title="Troubleshooting">

<PanelListItem number="1">

If Vue complains about linting, for instance with:

```
Syntax Error: Error: Failed to load config "prettier" to extend from.
Referenced from: /home/alice/checkers/vue/node_modules/eslint-plugin-prettier/eslint-plugin-prettier.js
```

You can safely tell it to ignore linting. To do that, in `package.json`, change the `"serve"` script to:

```json
{
    ...
    "scripts": {
        ...
        "serve": "vue-cli-service serve --skip-plugins @vue/cli-plugin-eslint",
    }
}
```

</PanelListItem>

<PanelListItem number="2" :last="true">

If `npm run serve` (Node v16) complains about `node-sass`, these versions should work in `package.json`:

```json
"dependencies": {
    "@starport/vue": "0.1.53",
    "@starport/vuex": "0.1.53",    
},
"devDependencies": {
    "node-sass": "^6.0.1",
    "sass-loader": "^10.0.0"
}
```

Do not forget to redo `npm install`.

In case you face a blank page, please look in the browser console for an error message.

If you see that `regeneratorRuntime` is missing, run `npm install --save regenerator-runtime` in the Vue folder and include `const regeneratorRuntime = require("regenerator-runtime");` into the `<script>` block of the `src/App.vue`.

</PanelListItem>

</ExpansionPanel>

Navigate to [localhost:8080](http://localhost:8080/). On the client-side, no wallets have been created or imported yet. Load Alice's wallet in the GUI to have some tokens. You will need to use the mnemonic for Alice, which you can find in the output of the `ignite chain serve` command. If you do not see the mnemonic, it is because the mnemonic was shown to you the first time you ran the command and you did not copy it. So reset with `ignite chain serve --reset-once`. Copy and paste it to _import a wallet_.

Now you should see the balance of Alice's account and can act on her behalf.

Select **Custom Type** in the sidebar to see custom types. There are no custom types yet, this page is empty for now.

<HighlightBox type="best-practice">

Make a Git commit before you create a new `message`. In fact, it is generally recommended to make a Git commit before running **any** `ignite scaffold` command. A Git commit protects the work you have done so far and makes it easier to see what the `scaffold` command added. It also makes it easy to just revert all changes if you are unsatisfied and want to run a different `scaffold` command.

</HighlightBox>

## Your first message

After your Git commit, create a simple `message` with:

```sh
$ ignite scaffold message createPost title body
```

The `ignite scaffold message` command accepts a message name (here `createPost`) as the first argument, and a list of fields for the message (here `title` and `body`), which are `string` fields unless mentioned otherwise.

A message is scaffolded in a module with a name that matches the name of the project by default. It is named `checkers` in this case. Alternatively you could have used `--module checkers`. Learn more about your options with:

```sh
$ ignite scaffold message --help
```

You can see a list of files that were created or modified by the `scaffold message` command in the Terminal output:

```
modify proto/checkers/tx.proto
modify x/checkers/client/cli/tx.go
create x/checkers/client/cli/tx_create_post.go
modify x/checkers/handler.go
create x/checkers/keeper/msg_server_create_post.go
modify x/checkers/types/codec.go
create x/checkers/types/message_create_post.go
```

The `modify` was made possible thanks to the lines like `// this line is used by starport scaffolding # 1` that you did not remove.

So where is everything? You can find the root definition of your new message in:

<CodeGroup>

<CodeGroupItem title="proto/checkers/tx.proto" active>

```protobuf
// this line is used by starport scaffolding # proto/tx/message
message MsgCreatePost {
  string creator = 1;
  string title = 2;
  string body = 3;
}
```

</CodeGroupItem>

</CodeGroup>

Ignite CLI also wired a new command into your chain's CLI in:

<CodeGroup>

<CodeGroupItem title="x/checkers/client/cli/tx_create_post.go" active>

```go
func CmdCreatePost() *cobra.Command {
  cmd := &cobra.Command{
    Use:   "create-post [title] [body]",
    Short: "Broadcast message createPost",
    Args:  cobra.ExactArgs(2),
    ...
  }
}
```

</CodeGroupItem>

</CodeGroup>

Ignite CLI scaffolded GUI elements relating to your message with a Vue.js frontend framework. You can, for instance, start with this function in:

<CodeGroup>

<CodeGroupItem title="vue/src/store/generated/alice/checkers/alice.checkers.checkers/index.ts" active>

```typescript
async MsgCreatePost({ rootGetters }, { value }) {
    try {
        const txClient=await initTxClient(rootGetters)
        const msg = await txClient.msgCreatePost(value)
        return msg
    } catch (e) {
        if (e == MissingWalletError) {
            throw new SpVuexError('TxClient:MsgCreatePost:Init', 'Could not initialize signing client. Wallet is required.')
        }else{
            throw new SpVuexError('TxClient:MsgCreatePost:Create', 'Could not create message: ' + e.message)

        }
    }
},
```

</CodeGroupItem>

<CodeGroupItem title="index.js">

```javascript
async MsgCreatePost({ rootGetters }, { value }) {
    try {
        const txClient = await initTxClient(rootGetters);
        const msg = await txClient.msgCreatePost(value);
        return msg;
    }
    catch (e) {
        if (e == MissingWalletError) {
            throw new SpVuexError('TxClient:MsgCreatePost:Init', 'Could not initialize signing client. Wallet is required.');
        }
        else {
            throw new SpVuexError('TxClient:MsgCreatePost:Create', 'Could not create message: ' + e.message);
        }
    }
}
```

</CodeGroupItem>

</CodeGroup>

## Next up

You just created a fully working Cosmos SDK chain, one that forms the basis of the [following exercise](./exercise-intro.md).

<HighlightBox type="info">

You can remove the `MsgCreatePost` message as it is not part of the guided exercise in the next sections. You can clean it all by running:

```sh
$ git checkout -f && git clean -fd
```

</HighlightBox>
