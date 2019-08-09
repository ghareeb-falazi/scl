# <ins>S</ins>mart <ins>C</ins>ontract <ins>L</ins>ocator (SCL)

**Version**: 1.0.0

**Date**: July 31, 2019

**Authors**:  
  Andrea Lamparelli  
  Ghareeb Falazi  
  Uwe Breitenbücher  
  Florian Daniel  
  Frank Leymann

## Introduction
This document specifies the Smart Contract Locator (SCL) extension to the URL scheme intended for the technology-agnostic addressing of permissioned and permissionless blockchain smart contracts.

## Scheme Definition
Given the [IETF specification](https://tools.ietf.org/html/rfc3986) of the generic URL format:
```
URL = scheme ":" [userinfo "@" ] host [":" port] path ["?" query] ["#" fragment]
```
An SCL is defined as a specialization of a URL composed of a standard URL (up to the `path` element included), which identifies a  gateway that provides access to the blockchain platform on which the designated smart contract is hosted, and of an SCL query, which identifies the target smart contract inside the blockchain network:
```
 SCL = scheme ":" [userinfo "@" ] host [":" port] path "?" scl_query
 scl_query = "blockchain=" bc "&blockchain-id=" id "&address=" addr

 bc = "ethereum" | "bitcoin" | "fabric" | "eosio" | ...

 id = NetworkIdentifier                    // not further detailed here

 addr = eth_addr | bit_addr | fab_addr | eos_addr | ...
 eth_addr = 40ByteHexString                // not further detailed here
 bit_addr = Bech32Address                  // not further detailed here
 fab_addr = PathString                     // not further detailed here
 eos_addr = 12CharacterString              // not further detailed here
 ...
```

Therefore, an SCL identifies:

1. Which type of blockchain is addressed via the `blockchain` query parameter
2. Which exact blockchain network is addressed (especially useful for permissioned blockchains where a gateway could connect to multiple instances of the same type).
3. The blockchain-internal smart contract address (technology-specific)

## Examples

In the following, we list example SCL addresses for a set of the supported blockchains that are accessed using the **https** scheme via a hypothetical gateway hosted at `mygateway.com`

### Ethereum

```
https://mygateway.com?blockchain=ethereum&blockchain-id=eth-mainnet
        &address=0xa0b73e1ff0b80914ab6fe0444e65848c4c34450b
```

### Bitcoin

```
https://mygateway.com?blockchain=bitcoin&blockchain-id=btc-mainnet
        &address=1Mbk53DzVKCz6MHiBd8ZHkPhsZETo7PtZR
```

### Hyperledger Fabric

```
https://mygateway.com?blockchain=fabric&blockchain-id=part-vendors
        &address=channel1%2Fchaincode1%2Fsmartcontract1
```

### EOSIO

```
https://mygateway.com?blockchain=eos&blockchain-id=eos-mainnet
        &address=myfancyacc05
```

## Notes

* The proposal of SCL is compliant with standard URLs, which makes it natively ready for the Internet. The examples here use a schema binding of **http** or **https**, but nothing prohibits the use of **SMTP** or any other transport protocol.
