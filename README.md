# Cartesi Machine Input/Output Codec

In the upcoming v2 release of the Cartesi Rollups SDK, inputs and outputs will be encoded as Solidity function calldata.
This repo aims to show front-end developers how inputs and outputs can be encoded or decoded using [`viem`](https://viem.sh/) and the contract ABIs exported by [`@cartesi/rollups`](https://www.npmjs.com/package/@cartesi/rollups).
It also comes with a handy-dandy CLI tool for encoding and decoding of inputs and outputs.

## Dependencies

- `pnpm`

## Setup

First, you'll need to install the Node dependencies.

```sh
pnpm i
```

## CLI

If you clone this repository, you can run `pnpm cmoic [args...]`.

If, instead, you install the package, you can just run `cmoic [args...]`.

## Encoding inputs

### To hex

```sh
pnpm cmioc encode input \
    --chain-id 1 \
    --app-contract 0x70ac08179605AF2D9e75782b8DEcDD3c22aA4D0C \
    --msg-sender 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266 \
    --block-number 42 \
    --block-timestamp 70000 \
    --index 10 \
    --payload 0xdeadbeef
```

Output:

```
0xcc7dee1f000000000000000000000000000000000000000000000000000000000000000100000000000000000000000070ac08179605af2d9e75782b8decdd3c22aa4d0c000000000000000000000000f39fd6e51aad88f6f4ce6ab8827279cfffb92266000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000011170000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000004deadbeef00000000000000000000000000000000000000000000000000000000
```

### To binary

Just add the `--binary` flag.

## Decoding inputs

### From hex

```sh
pnpm cmioc decode input 0xcc7dee1f000000000000000000000000000000000000000000000000000000000000000100000000000000000000000070ac08179605af2d9e75782b8decdd3c22aa4d0c000000000000000000000000f39fd6e51aad88f6f4ce6ab8827279cfffb92266000000000000000000000000000000000000000000000000000000000000002a0000000000000000000000000000000000000000000000000000000000011170000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000004deadbeef00000000000000000000000000000000000000000000000000000000
```

or

```sh
cat input.txt | pnpm cmioc decode input
```

### From binary

```sh
cat input.bin | pnpm cmioc decode input --binary
```

Output:

```json
{
    "chainId": "1",
    "appContract": "0x70ac08179605AF2D9e75782b8DEcDD3c22aA4D0C",
    "msgSender": "0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266",
    "blockNumber": "42",
    "blockTimestamp": "70000",
    "index": "10",
    "payload": "0xdeadbeef"
}
```

## Encoding outputs

### Notices

#### To hex

```sh
pnpm cmioc encode notice \
    --payload 0xdeadbeef
```

Output:

```
0xc258d6e500000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000004deadbeef00000000000000000000000000000000000000000000000000000000
```

#### To binary

Just add the `--binary` flag.

### Vouchers

#### To hex

```sh
pnpm cmioc encode voucher \
    --destination 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266 \
    --value 1000000000000000000 \
    --payload 0xfafafa
```

Output:

```
0x237a816f000000000000000000000000f39fd6e51aad88f6f4ce6ab8827279cfffb922660000000000000000000000000000000000000000000000000de0b6b3a764000000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000003fafafa0000000000000000000000000000000000000000000000000000000000
```

#### To binary

Just add the `--binary` flag.

## Decoding outputs

### Notices

#### From hex

```sh
pnpm cmioc decode output 0xc258d6e500000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000004deadbeef00000000000000000000000000000000000000000000000000000000
```

or

```sh
cat notice.txt | pnpm cmioc decode output
```

#### From binary

```sh
cat notice.bin | pnpm cmioc decode output --binary
```

Output:

```json
{
    "type": "notice",
    "payload": "0xdeadbeef"
}
```

### Vouchers

#### From hex

```sh
pnpm cmioc decode output 0x237a816f000000000000000000000000f39fd6e51aad88f6f4ce6ab8827279cfffb922660000000000000000000000000000000000000000000000000de0b6b3a764000000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000003fafafa0000000000000000000000000000000000000000000000000000000000
```

or

```sh
cat voucher.txt | pnpm cmioc decode output
```

#### From binary

```sh
cat voucher.bin | pnpm cmioc decode output --binary
```

Output:

```json
{
    "type": "voucher",
    "destination": "0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266",
    "value": "1000000000000000000",
    "payload": "0xfafafa"
}
```

## Tests

You can run the tests with the following command

```sh
pnpm test
```
