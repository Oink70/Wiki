# Question: I accidentally send funds to a B-address and cannot move those funds

`verus createrawtransaction '[{"txid": "yourtxid here", "vout": fill in too}]' "{"destination addr": 48"}" <any recent block height here, i.e. current -5 or whatever>`

1) adapt the above `createrawtransaction` api call
2) `signrawtransaction` the result of 1)
3) `sendrawtransaction` the result of 2
