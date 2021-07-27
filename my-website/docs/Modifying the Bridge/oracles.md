---
sidebar_position: 1
---

If you want to change the code....

## Customizing Docker Image

If you want to customize the docker image feel free - the executable is managed by supervisor, the configuration of which can be found in deployment/config/supervisor.conf. You can add a PKCS11 module by adding installation of the client module to the docker image, and overriding the pkcs11_module environment variable to point to the library .so file. Other HSM or key vault support can be added by request

To compile a new docker image, using our handy dockerfile:

docker build -f deployment/dockerfiles/bridge.Dockerfile -t enigmampc/eth-bridge-test:0.0.1 .

And push it to dockerhub (or some registry)

docker push enigmampc/eth-bridge-test:0.0.1


## Customizing Parameters

Set all the configuration parameters you can before building (you can always override them with environment variables). Specifically make sure you set these (or at least check that the set defaults are okay) -
•	"signatures_threshold"
•	"eth_confirmations"
•	"eth_start_block"
•	"sleep_interval"
•	"network"
•	"chain_id"

Also, you should have the following values set from the previous steps -
•	multisig_acc_addr
•	multisig_wallet_address
•	scrt_swap_address
•	swap_code_hash

Now go to the machine that’s going to run the signer/leader/whatever. You’ll need 3 things -

1.	Ethereum private key + address (either in bytes, or pkcs8)
2.	Secret private key file + password
3.	id_tx_io.json - key that is used to encrypt transactions.

The first two you should have available from the previous steps. id_tx_io.json is generated automatically in secretcli (you can find it in ~/.secretcli/id_tx_io.json). This key must be shared between all parties, since the raw multisig transaction is encrypted and without this key the signers will not be able to decrypt the message.

I recommend deploying using a docker-compose file, although it is up to the reader to decide how to deploy the docker container. Either way, the file here shows the configuration parameters you need to set.
Note: If you want to use PKCS#11 - softhsm has been tested and works well (though I haven’t figured out if exporting/importing keys works, so before that I’m not going to use it in production), other pkcs#11 providers should be okay as well (AWS CloudHSM etc.), but I haven’t tested it. To get it to run inside the container there would probably have to be some dependencies installed as well (the client library).

Once you’ve set everything up to your liking, simply start the container and let it do it’s thing!
