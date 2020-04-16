# Mini taller de Substrate


## Bloques

Obtener informacion de un bloque, incluyendo extrinsics (inherents + transactions)

```javascript
const blockNumber = 1
const blockHash = await api.rpc.chain.getBlockHash(blockNumber)
const block = await api.rpc.chain.getBlock(blockHash)
console.log(JSON.stringify(block, null, 2))
```

## Extrinsics

"A piece of data bundled into a block that expresses something from the "external" (i.e. off-chain) world. There are, broadly speaking, two types of extrinsic: transactions (which tend to be signed) and inherents (which don't)."

Obtener extrinsics de un bloque:

```javascript
const blockNumber = 1798661
const blockHash = await api.rpc.chain.getBlockHash(blockNumber)
const { block } = await api.rpc.chain.getBlock(blockHash)
console.log(JSON.stringify(block.extrinsics.toHuman(), null, 2))
```

Listado de extrinsics: https://polkadot.js.org/api/substrate/extrinsics.html

## Transacciones

"A type of Extrinsic that includes a signature, and all valid instances cost the signer some amount of tokens when included on-chain. Because validity can be determined efficiently, transactions can be gossipped on the network with reasonable safety against DoS attacks, much as with Bitcoin and Ethereum."

Obtener transacciones de un bloque:

```javascript
const blockNumber = 1798661
const blockHash = await api.rpc.chain.getBlockHash(blockNumber)
const { block } = await api.rpc.chain.getBlock(blockHash)
block.extrinsics.forEach((extrinsic, index) => {
  if (extrinsic.isSigned) {
    console.log(extrinsic.hash.toHex(), JSON.stringify(extrinsic.toHuman(), null, 2))
  }
})
```

Ver transacciones en PolkaScan: https://polkascan.io/pre/kusama/transaction

**Curiosidad:** En Substrate también tenemos transacciones multimensaje como en Tendermint, solo que aquí se llaman batch: https://polkascan.io/pre/kusama/transaction?module=utility&call=batch&page=1


## Inherents

Los `inherents` como hemos visto son extrinsics que no van firmadas, se usan para unmutabilizar información que por su naturaleza no precisa de un alto nivel de seguridad.

El mejor ejemplo de inherent es la llamada `set` del método `timestamp`, que almacena el timestamp del último bloque.

Otro ejemplo es la llamada `final_hint` del método `finalitytracker`, en el que el autor del último bloque propone para su almacenaje el número de bloque finalizado actual.

Ver inherents en PolkaScan: https://polkascan.io/pre/kusama/inherent

## Eventos

Todos los extrinsics generan al menos un evento (ExtrinsicSuccess/ExtrinsicFailed), aunque por lo general las transacciones generan mas de uno, por ejemplo, una transferencia de fondos exitosa (balance/transfer) generará al menos 5 eventos:

* Deposito fee en el tesoro
* Deposito fee para el autor del bloque (tip)
* La transferencia de fondos en si misma
* Evento de éxito

Ejemplo: https://polkascan.io/pre/kusama/transaction/0x26f6929fa31a405ef0055ff291afbe2f95e7fafba318fcd59b2b6c58396b9f34

Para subscribirnos a los eventos del sistema: https://polkadot.js.org/api/examples/promise/08_system_events/

## Recursos

### Polkadot API doc

https://polkadot.js.org/api/

### API playground

https://polkadot.js.org/apps/#/js

### Substrate doc

https://substrate.dev/docs/en/
