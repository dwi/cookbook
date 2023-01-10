# Python considerations

Most of Web3.js examples work using [Web3.py](https://web3py.readthedocs.io/en/stable/) but some care should be taken when signing a message using EIP-712 (for the signature of CreateOrder GraphQL endpoint for example): v0.7.0+ of eth-account should be used otherwised it won't work.

### Signing a message with EIP-712 <a href="#signing-a-message-with-eip-712" id="signing-a-message-with-eip-712"></a>

```python
from web3 import Web3
from eth_account.messages import encode_structured_data

signed_msg = Web3().eth.account.sign_message(
                encode_structured_data(data_as_dict),
                private_key=private_key)
```

​
