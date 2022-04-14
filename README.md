# One Did Provider

提供自定义标识的DID

### Installation
```
npm install @aomi-blockchain/one-did-provider
// or
yarn add @aomi-blockchain/one-did-provider
```

### Providers

#### OneEd25519Provider

基于`OneEd25519Provider`封装的DID。

原ED25519 DID 结构
```
did:key:z${base58(publicKey)}
```
One ED25519 DID 结构
```
did:one:${base64(flag)}.z${base58(publicKey)}
```

```typescript
import { OneEd25519Provider } from '@aomi/one-did-provider'
import KeyResolver from 'key-did-resolver'
import { DID } from 'dids'

const seed = new Uint8Array(...) //  32 bytes with high entropy
const provider = new OneEd25519Provider('master app',seed)
const did = new DID({ provider, resolver: KeyResolver.getResolver() })
await did.authenticate()

// log the DID
console.log(did.id)

// create JWS
const { jws, linkedBlock } = await did.createDagJWS({ hello: 'world' })

// verify JWS
await did.verifyJWS(jws)

// create JWE
const jwe = await did.createDagJWE({ very: 'secret' }, [did.id])

// decrypt JWE
const decrypted = await did.decryptDagJWE(jwe)

```

