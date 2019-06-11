**********
Quickstart
**********

Just want to get a name and make it resolve to something? Here's how.

First, download `ensutils.js` to your local machine, and import it into an PUFFScoin console on a node synced to ropsten or rinkeby:

::

    loadScript('/path/to/ensutils.js');

Before registering, check that nobody owns the name you want to register:

::

    new Date(Registrar.expiryTimes(web3.sha3('myname')).toNumber() * 1000)

If this line returns a date earlier than the current date, the name is available and you're good to go. You can register the domain for yourself by running:

::

    Registrar.register(web3.sha3('myname'), puffs.accounts[0], {from: puffs.accounts[0]})

Next, tell the PUFFScoin-ENS registry to use the public resolver for your name:

::

    ens.setResolver(namehash('myname.puffs'), publicResolver.address, {from: puffs.accounts[0]});

Once that transaction is mined, tell the resolver to resolve that name to your account:

::

    publicResolver.setAddr(namehash('myname.puffs'), puffs.accounts[0], {from: puffs.accounts[0]});

...or any other address:

::

    publicResolver.setAddr(namehash('myname.puffs'), '0x1234...', {from: puffs.accounts[0]});

If you want, create a subdomain and do the whole thing all over again:

::

    ens.setSubnodeOwner(namehash('myname.puffs'), web3.sha3('whatis'), puffs.accounts[1], {from: puffs.accounts[0]});
    ens.setResolver(namehash('foo.myname.puffs'), publicResolver.address, {from: puffs.accounts[1]});
    ...

Finally, you can resolve your newly created name:

::

    getAddr('myname.puffs')

which is shorthand for:

::

    resolverContract.at(ens.resolver(namehash('myname.puffs'))).addr(namehash('myname.puffs'))

.. _ensutils.js: https://github.com/puffscoin/ens/blob/master/ensutils.js
.. _ensutils-testnet.js: https://github.com/puffscoin/ens/blob/master/ensutils-testnet.js
