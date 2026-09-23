# improv-serial

A small `no_std` Rust implementation of the [Improv Serial protocol](https://www.improv-wifi.com/serial/).

The crate contains only protocol framing and parsing. It does **not** depend on ESP hardware, a UART/USB driver, Wi-Fi, Embassy, or any application framework.

It was extracted from [`embewi-agent-esp`](https://github.com/iobewi/embewi-agent-esp), where the same codec is used with ESP Web Tools provisioning.

## Scope

- byte-by-byte Improv Serial RPC parser;
- Wi-Fi settings decoding;
- current-state and error frames;
- RPC response frames;
- silent stream resynchronization when unrelated bytes/log output are present;
- `no_std + alloc`;
- host-side unit tests.

Transport ownership deliberately remains with the caller.

## Example

```rust
use improv_serial::{ParsedCommand, Parser};

let mut parser = Parser::new();

for byte in serial_bytes {
    if let Some(command) = parser.feed(byte) {
        if let ParsedCommand::WifiSettings(settings) = command {
            // Connect using settings.ssid / settings.password.
        }
    }
}
```

## Status

The API is intentionally small and currently reflects the Improv Serial surface needed by its first real consumer. Treat it as pre-stable until additional applications exercise the abstraction.

## License

MIT.
