# Snipcart API Keys

How it works : 

* two keys
* one is "public" : it is available unencrypted in the javasript in the client browser. It allows to use just one endpoint, the "pay" endpoint. Well if someone thinks paying me is a good hack, let's just hoped we'll have a lot of hackers like that. Any other APi operation is forbidden, when using this key to authenticate to the Snipcart REST API
* one is "secret" : the owner keeps it for himself, it gives access to all of the features of the Snipcart API : that is , youcan do just about anything on your Snipcat account, if you authenticate using this key.

So those two keys are roles, in a way.
