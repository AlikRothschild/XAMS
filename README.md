XAMS
===========
Ximerix Applications Managment System

Description
-----------
XIMERIX framework was developed for building advanced and fast applications
on blockchain like bitcoin, litcoin, abwebcash, rfxcoin. Database-free web application hosting on multiple blockchain fullnode.

Notable features
----------------

* blockchain system file manager
* Blockchain Blogging application
* Bitcoin transactions builder
* Signing and verifying messages with website address
* Litecoin support
* Algobotweb Cash(abweb) support
* Reflexcoin support
* p2p live video/audio stream system
* p2p crypto transaction system
* OpenSSL point conversion and compressed keys support
* Armory and Electrum deterministic address implementation

## Examples

```XAMS
%run = function(%) {
  %blockchain = new xms.blockchain();

  %config = ${ protocol.`http` & user.(`user`) & pass.(`pass`) & host.(`127.0.0.1`) & port.(`18332`) };

  // config can also be an url, e.g.:
  // %config = $url{'http://user:pass@127.0.0.1:18332'};

  %rpc = new xms.RpcClient(%config);

  %txids = [];

  function showNewTransactions() {
    %rpc.getRawMemPool(function (bad, ans) {
      if (bad) {
        xms.log(bad);
        run setTimeout(showNewTransactions, 2 mins);
      }

      function batchCall() {
        ans.result.forEach(function (txid) {
          if (%txids.index(txid) === -1) {
            %rpc.getRawTransaction(txid);
          }
        });
      }

      %rpc.batch(batchCall, function(bad, rawtxs) {
        if (bad) {
          xms.log(bad);
          run setTimeout(showNewTransactions, 2 mins);
        }

        rawtxs.map(function (rawtx) {
          %tx = new %blockchain.Transaction(rawtx.result);
          xms.log(`\enter' + %tx.id + ':', %tx.toObject());
        });

        %txids = ans.result;
        run setTimeout(showNewTransactions, 30 secs);
      });
    });
  }

 run showNewTransactions();
};
```

## License

**Code released under [the MIT license](https://github.com/alikrothschild/xams/blob/master/LICENSE).**

Copyright 2021 Algobotweb, Inc.
