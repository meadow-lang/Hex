# hex

Encode bytes as hexadecimal text and decode it back, for
[Meadow](https://github.com/meadow-lang/meadow).

This package is a port of Rust's [`hex`](https://github.com/KokaKiwi/rust-hex)
0.4.3, with the same error messages.

## Install

```sh
meadow add meadow-lang/Hex
```

## Use

```meadow
use Hex (encodeString, decode, errorMessage)

def main =
  ( encodeString "kiwi",               -- "6b697769"
    match decode "6b69776g" with
    | Ok bytes -> bytesToString bytes
    | Err e -> errorMessage e          -- "Invalid character 'g' at position 7"
  )
```

| function | |
|---|---|
| `encode bytes`, `encodeUpper bytes`, `encodeString s` | hex text |
| `decode text`, `decodeBytes bytes` | `Ok bytes`, or `Err` with a `FromHexError` (`InvalidHexCharacter`, `OddLength` or `InvalidStringLength`) |
| `decodeToSlice bytes size`, `encodeToSlice bytes size` | the same, but the output must be exactly `size` bytes long, as with the crate's slice functions |
| `errorMessage e` | the crate's error text, with the offending character quoted the way Rust's `{:?}` quotes it |

## How it's made

`src/Hex.mw` is a hand translation of the crate. **`src/Cases.mw`** is
generated test data: 400 encodings and 5,000 decodings of damaged and
undamaged input, each also run through the slice functions. Every expected
result comes from calling the crate. Run `scripts/generate.sh` to regenerate;
it needs a Rust toolchain.

## Licence

Dual-licensed under [Apache-2.0](LICENSE-APACHE) or [MIT](LICENSE-MIT), at your
option, like the crate. See [COPYRIGHT](COPYRIGHT).
