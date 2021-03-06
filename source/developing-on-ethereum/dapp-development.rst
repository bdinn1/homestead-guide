********************************************************************************
ÐApp Development
********************************************************************************

Four Primary Resources
================================================================================
ÐApp development requires an understanding of the Web3 Javascript API, the JSON RPC API, and the Solidity programming language. Note: There are developer tools that help you develop, test, and deploy ÐApps in a way that automatically utilizes the resources listed below.

.. todo::
   Add cross reference to developer tools page in the first paragraph.

-  `Web3 JavaScript API <https://github.com/ethereum/wiki/wiki/JavaScript-API>`__ - This is the main JavaScript SDK to use when you want to interact with an Ethereum node.
-  `JSON RPC API <https://github.com/ethereum/wiki/wiki/JSON-RPC>`__ - This is the low level JSON RPC 2.0 interface to interface with a node. This API is used by the `Web3 JavaScript API <https://github.com/ethereum/wiki/wiki/JavaScript-API>`__.
-  `Solidity Documentation <https://solidity.readthedocs.org/en/latest/>`__ - Solidity is the Ethereum developed Smart Contract language, which compiles to EVM (Ethereum Virtual Machine) opcodes.
-  Testnets - Test networks help developers develop and test Ethereum code and network interactions without spending their own Ether on the main network. Test network options are listed below.

Connecting to Morden Testnet
================================================================================
Morden is a public Ethereum alternative testnet. It is expected to
continue throughout the Frontier and Homestead milestones of the software.

Usage
--------------------------------------------------------------------------------

TurboEthereum (C++)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This is supported natively on 0.9.93 and above. Pass the ``--morden`` argument in when starting any of the clients. e.g.:

.. code:: Console

   > eth --morden

Or, for AlethZero

.. code:: Console

   > alethzero --morden

PyEthApp (Python client)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

PyEthApp supports the morden network from v1.0.5 onwards:

.. code:: Console

   > pyethapp --profile morden run

geth (Go client)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: Console

   > geth --testnet

Details
--------------------------------------------------------------------------------
All parameters are the same as Frontier except:

-  Network Name: **Morden**
-  Network Identity: 2
-  genesis.json (given below);
-  Initial Account Nonce (``IAN``) is 2^20 (instead of 0 in all previous
   networks).

   -  All accounts in the state trie have nonce >= ``IAN``.
   -  Whenever an account is inserted into the state trie it is
      initialised with nonce = ``IAN``.

-  Genesis block hash:
   ``0cd786a2425d16f152c658316c423e6ce1181e15c3295826d7c9904cba9ce303``
-  Genesis state root:
   ``f3f4696bbf3b3b07775128eb7a3763279a394e382130f27c21e70233e04946a9``

Morden's genesis.json
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: JSON

    {
            "nonce": "0x00006d6f7264656e",
            "difficulty": "0x20000",
            "mixhash": "0x00000000000000000000000000000000000000647572616c65787365646c6578",
            "coinbase": "0x0000000000000000000000000000000000000000",
            "timestamp": "0x00",
            "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
            "extraData": "0x",
            "gasLimit": "0x2FEFD8",
            "alloc": {
                    "0000000000000000000000000000000000000001": { "balance": "1" },
                    "0000000000000000000000000000000000000002": { "balance": "1" },
                    "0000000000000000000000000000000000000003": { "balance": "1" },
                    "0000000000000000000000000000000000000004": { "balance": "1" },
                    "102e61f5d8f9bc71d0ad4a084df4e65e05ce0e1c": { "balance": "1606938044258990275541962092341162602522202993782792835301376" }
            }
    }

Getting Morden Testnet Ether
--------------------------------------------------------------------------------

Three ways to get Morden testnet ether:

- You can mine testnet Ethereum
- Use the `Ethereum wei faucet <https://zerogox.com/ethereum/wei_faucet>`__. 

.. todo::
   Finish Morden Testnet Section


Setting Up a Local Private Testnet
================================================================================
You either pre-generate or mine your own Ether on a private
testnet. It is a much more cost effective way of trying out
Ethereum.

The things that are required to specify in a private chain are:
 - Custom Genesis File
 - Custom Data Directory
 - Custom NetworkID
 - (Recommended) Disable Node Discovery

The Genesis File
--------------------------------------------------------------------------------

The Genesis block is the start of the Blockchain - the first
block, block 0, and the only block that does not point to a predecessor
block. Ethereum’s client protocol ensures that no other node will agree with your version of the
blockchain unless they have the same genesis block, so you can make as many private testnet blockchains as you'd like!

CustomGensis.json

.. code-block:: JSON

  {   
      "nonce": "0x0000000000000042",     "timestamp": "0x0",     
      "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",     
      "extraData": "0x0",     "gasLimit": "0x8000000",     "difficulty": "0x400",     
      "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",     
      "coinbase": "0x3333333333333333333333333333333333333333",     "alloc": {     }
  }

Save a file called CustomGenesis.json.
You will reference this when starting your geth node using the following flag:

``--genesis /path/to/CustomGenesis.json``

Geth Flags For Your Private Network
--------------------------------------------------------------------------------

There are some command line options (also called “flags”) that are
necessary in order to make sure that your network is private. We already covered the genesis flag, but we need a few more. 

``--nodiscover``

Use this to make sure that your node is not discoverable by people who do not manually add you. Otherwise, there is a chance that your node may be inadvertently added to a stranger's blockchain if they have the same genesis file and network id.

``--maxpeers 0``

Use maxpeers 0 if you do not want anyone else connecting to your test chain. Alternatively, you can adjust this number if you know exactly how many peers you want connecting to your node.

``--rpc``

This will enable RPC interface on your node. This is generally enabled by default in Geth.


``--rpcapi "db,eth,net,web3"``

This dictates what APIs that are allowed to be accessed over RPC. By default, Geth enables the web3 interface over RPC. 

**IMPORTANT: Please note that offering an API over the RPC/IPC interface will give everyone access to the API who can access this interface (e.g. ÐApp's). Be careful which API's you enable. By default geth enables all API's over the IPC interface and only the db,eth,net and web3 API's over the RPC interface.**

``--rpcport "8080"``

Change 8000 to any port that is open on your network. The default for geth is 8080.

``--rpccorsdomain "http://chriseth.github.io/browser-solidity/"``

This dictates what URLs can connect to your node in order to perform RPC client tasks. Be very careful with this and type a specific URL rather than the wildcard (*) which would allow any URL to connect to your RPC instance.


``--datadir "/home/TestChain1"``

This is the data directory that your private chain data will be stored in. Choose a location that is separate from your public Ethereum chain folder.


``--port "30303"``

This is the "network listening port", which you will use to connect with other peers manually.


``--identity "TestnetMainNode"``

This will set up an identity for your node so it can be identified more easily in a list of peers.
Here is an example of how these identities show up on the network.


Creating the geth Command
--------------------------------------------------------------------------------

After you have created your custom genesis block JSON file and created a directory for your blockchain data, type the following command into your console that has access to geth:

.. code-block:: Console

  geth --identity "MyNodeName" --genesis /path/to/CustomGenesis.json --rpc --rpcport "8080" --rpccorsdomain "*" --datadir "C:\chains\TestChain1" --port "30303" --nodiscover --rpcapi "db,eth,net,web3" --networkid 1999 console

**Note:** Please change the flags to match your custom settings.

You will need to start your geth instance with your custom chain command every time you want to access your custom chain. If you just type "geth" in your console, it will not remember all of the flags you have set.

Pre-Allocating Ether to Your Account
--------------------------------------------------------------------------------

A difficulty of "0x400" allows you to mine Ether very quickly on your private testnet chain. If you create your chain and start mining, you should have hundreds of Ether in a matter of minutes which is way more than enough to test transactions on your network. If you would still like to pre-allocate Ether to your account, you will need to:

1. Create a new Ethereum account after you create your private chain
2. Copy your new account address
3. Add the following command to your Custom_Genesis.json file:

.. code-block:: Javascript

  "alloc":
  { 
	  "<your account address e.g. 0x1fb891f92eb557f4d688463d0d7c560552263b5a>":
	  { "balance": "20000000000000000000" } 
  }

**Note:** Replace ``0x1fb891f92eb557f4d688463d0d7c560552263b5a`` with your account address.

Save your genesis file and re-run your private chain command. Once geth is fully loaded, close Geth.

We want to assign an address as "primary" and check it's balance.

Run the command ``geth account list`` in your console to see what account # your new address was assigned.

.. code-block:: Console

   > geth account list
   Account #0: {d1ade25ccd3d550a7eb532ac759cac7be09c2719}
   Account #1: {da65665fc30803cb1fb7e6d86691e20b1826dee0}
   Account #2: {e470b1a7d2c9c5c6f03bbaa8fa20db6d404a0c32}
   Account #3: {f4dd5c3794f1fd0cdc0327a83aa472609c806e99}
   
Take note of which account # is the one that you pre-allocated Ether to.

.. code-block:: Console

  > primary = eth.accounts[0];

**Note:** Replace ``0`` with your account's number.
This console command should return your primary Ethereum address. 

Type the following command:

.. code-block:: Console

  > balance = web3.fromWei(eth.getBalance(primary), "ether");

This should return you ``20`` Ether in your account. The reason we had to put such a large number in the alloc section of your genesis file is because the "balance" field takes a number in wei which is the smallest sub-unit of Ether.