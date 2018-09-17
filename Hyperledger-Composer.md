# Hyperledger Composer

## Generate a business network archive.

```shell
composer archive create -t dir -n .
```
```shell
--sourceType, -t The type of the input containing the files used to create the archive  [required] [choices: "module", "dir"]
```
```shell
--sourceName, -n The Location to create the archive from e.g. NPM module directory or Name of the npm module to use  [required]
```

## Deploying the business network

After the business network has been installed, the network can be started. For best practice, a new identity should be created to administer the business network after deployment. This identity is referred to as a **network admin**.

### To install the business network

- Deploying a business network to the Hyperledger Fabric requires the Hyperledger Composer business network to be installed on the peer.
- The network administrator business network card must be imported for use.

```shell
composer network install --card PeerAdmin@hlfv1 --archiveFile network-name@0.0.1.bna
```
**PeerAdmin@hlfv1** is the network administrator business network card.

```shell
--card, -c The cardname to use to install the network  [string] [required]
```

```shell
--archiveFile, -a  The business network archive file name  [string] [required]
```

### To start the business network

```shell
composer network start --networkName network-name --networkVersion 0.0.1 --networkAdmin admin --networkAdminEnrollSecret adminpw --card PeerAdmin@hlfv1 --file networkadmin.card
```

```shell
--networkName, -n Name of the business network to start  [required]
```

```shell
--networkVersion, -V Version of the business network to start  [required]
```

```shell
--networkAdmin, -A The identity name of the business network administrator  [string] [required]
```

```shell
--networkAdminEnrollSecret, -S The enrollment secret for the business network administrator  [string]
```

```shell
--card, -c The cardname to use to start the network  [string] [required]
```

```shell
--file, -f File name of the card to be created  [string]
```

### To import the network administrator identity as a usable business network card

```shell
composer card import --file networkadmin.card
```

```shell
--file, -f The card file name  [string] [required]
```

### To check that the business network has been deployed successfully

```shell
composer network ping --card admin@network-name
```

```shell
--card, -c The cardname to use to ping the network  [string] [required]
```

## Generate a REST server

```shell
composer-rest-server
```
- Enter **admin@network-name** as the card name.

### To restart the REST server using the same options

```shell
composer-rest-server -c admin@network-name -n never -w true
```

```shell
-c, --card The name of the business network card to use  [string]
```

```shell
-n, --namespaces Use namespaces if conflicting types exist  [string] [choices: "always", "required", "never"] [default: "always"]
```

```shell
-w, --websockets Enable event publication over WebSockets  [boolean] [default: true]
```

## Generating an Angular 4 Application

```shell
yo hyperledger-composer:angular
```

## Upgrading a Deployed Network

1. Open `package.json`. Increment the `version` property. For example, if the current `version` is `"0.0.1"`, you can increment it by 1 to `"0.0.2"` (this uses the semantic version framework, [see](https://semver.org/)).
2. Repackage the business network into a business network archive (.bna). Remember to change the version number of the new .bna file.
    ```shell
    composer archive create --sourceType dir --sourceName . -a network-name@0.0.2.bna
    ```

    ```shell
    --sourceType, -t The type of the input containing the files used to create the archive  [required] [choices: "module", "dir"]
    ```

    ```shell
    --sourceName, -n The Location to create the archive from e.g. NPM module directory or Name of the npm module to use  [required]
    ```

    ```shell
    --archiveFile, -a Business network archive file name. Default is based on the Identifier of the BusinessNetwork [string]
    ```

## Deploy the update business network definition

### Install the updated business network

```shell
composer network install --card PeerAdmin@hlfv1 --archiveFile network-name@0.0.2.bna
```

### Upgrade the network to the new version

```shell
composer network upgrade -c PeerAdmin@hlfv1 -n network-name -V 0.0.2
```

### Check the current version of the business network

```shell
composer network ping -c admin@network-name | grep Business
```

## Hyperledger Composer Administrator

[link to tutorial](https://www.ibm.com/developerworks/cloud/library/cl-deploy-interact-extend-local-blockchain-network-with-hyperledger-composer/index.html)

There are two levels of security with Hyperledger Composer:
- Hyperledger Fabric administrator
- Business network administrator

The administrator for your local Hyperledger Fabric is the Peer Administrator (or PeerAdmin for short), which you created when you installed the local Hyperledger Fabric. Every business network should have an administrator as well, which is created when the network is deployed by the Hyperledger Fabric administrator. Authentication for both is handled using ID Cards.

## ID Cards

An ID card (or *card* for short) is a collection of files that contains all the information necessary to allow a participant to connect to a business network. The card is referred to as an *identity*. Before it can be used, it must be issued to the user, allowing him or her to be authenticated and autherised to use the network. Cards are a very handy way of keeping up with a password (called a *secret* in Hyperledger Composer terminology), you import the card into a collection of cards in the Hyperledger Fabric called a *wallet*. From that point on, you can just reference the card to authenticate that identity.

The general form for specifying a card is: *userid@network*, where *userid* is the user's unique id, and the *network* is the network to which the user is authenticated.

To handle the two levels of Hyperledger Composer security, you need a card for at least: (1) the PeerAdmin and (2) the business network admin.

### PeerAdmin

The *PeerAdmin* card is special ID card used to administer the local Hyperledger Fabric. In a development installation, such as the one on your computer, the PeerAdmin ID card is created when you install the local Hyperledger Fabric.

The form for a *PeerAdmin* card for a Hyperledger Fabric v1.0 network is *PeerAdmin@hlfv1*.

In general, the PeerAdmin is a special role reserved for functions such as:
- Deploying business networks
- Creating, issuing, and revoking ID cards for business network admins

As a developer, in a production Hyperledger Fabric installation, you would not have access to the *PeerAdmin* card. Instead the Hyperledger Fabric administrator would deploy your business network, create ID cards, and so on. When developing and testing blockchain networks using your local Hyperledger Fabric, you will use the PeerAdmin ID card to perform these functions.

### Business network admin

When the PeerAdmin deploys your network to the Hyperledger Fabric, an ID card is issued to the business network administrator, and then this card is used whenever the business network adminstrator needs to do anything with the business network, such as using the Composer Command Line Interface (CLI).

A general form of a business network admin card is *admin@network-name*.

In general, the business network admin is a special role reserved for functions such as:
- Updating the running business network
- Querying the various registries (participant, identity, and so forth)
- Creating, issuing, and revoking ID cards for participants in the business network.

## Wallet and Business Network Cards

Hyperledger Composer can only use business network cards that are placed into a wallet. The wallet is a directory on the file system that contains business network cards.

```shell
composer card import -f AdminName@network-name.card
```

```shell
--file, -f The card file name  [string] [required]
```