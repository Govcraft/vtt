# VTT

VTT is a Rust library for parsing and writing WebVTT (Web Video Text Tracks)
files. It helps you create, edit, and manage WebVTT cues, timestamps, and
settings. VTT integrates seamlessly with [Serde](https://serde.rs/) for
efficient data handling within Rust applications.

## Features

- **Parse WebVTT Files:** Convert WebVTT files into Rust data structures.
- **Write WebVTT Files:** Convert Rust data back to WebVTT format.
- **Manage Cues:** Add, modify, and arrange WebVTT cues.
- **Handle Timestamps:** Work with precise timestamps for video tracks.
- **Use with Serde:** Easily serialize and deserialize VTT data structures using
  Serde.

## Installation

Add VTT to your project's `Cargo.toml`:

```toml
[dependencies]
vtt = "1.0"
serde = { version = "1.0", features = ["derive"] }
```

## Usage

Import the prelude to access common types:

```rust
use vtt::prelude::*;
use std::time::Duration;

fn main() {
    let mut vtt = WebVtt::new();
    vtt.add_metadata("Language", "en-US");

    let cue = VttCue {
        identifier: Some("1".to_string()),
        start: VttTimestamp::new(Duration::from_secs(1)),
        end: VttTimestamp::new(Duration::from_secs(5)),
        settings: None,
        payload: "Hello, world!".to_string(),
    };
    vtt.add_cue(cue);

    println!("{}", vtt);
}
```

### Parsing a WebVTT File

Convert a WebVTT string into a `WebVtt` instance:

```rust
use vtt::prelude::*;
use std::str::FromStr;

fn parse_vtt(content: &str) -> Result<WebVtt, VttParseError> {
    WebVtt::from_str(content)
}

fn main() {
    let content = "WEBVTT

00:01:02.000 --> 00:03:04.000
Hello, world!";
    let vtt = parse_vtt(content).unwrap();
    println!("Number of cues: {}", vtt.cues.len());
}
```

### Writing to WebVTT

Convert a `WebVtt` instance to a WebVTT string:

```rust
use vtt::prelude::*;
use std::time::Duration;

fn main() {
    let mut vtt = WebVtt::new();
    vtt.add_metadata("Language", "en-US");

    let cue = VttCue {
        identifier: Some("1".to_string()),
        start: VttTimestamp::new(Duration::from_secs(1)),
        end: VttTimestamp::new(Duration::from_secs(5)),
        settings: None,
        payload: "Hello, world!".to_string(),
    };
    vtt.add_cue(cue);

    let serialized = vtt.to_string();
    println!("{}", serialized);
}
```

### Serialization and Deserialization with Serde

The VTT library fully supports serialization and deserialization of VTT domain
types using Serde. This integration allows you to efficiently convert WebVtt
structures to and from their WebVTT-formatted string representations,
facilitating easy manipulation and management of subtitle data within Rust
applications.

**Serializing a `WebVtt` instance:**

```rust
use vtt::prelude::*;
use std::time::Duration;
use serde::Serialize;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let mut vtt = WebVtt::new();
    vtt.add_metadata("Language", "en-US");

    let cue = VttCue {
        identifier: Some("1".to_string()),
        start: VttTimestamp::new(Duration::from_secs(1)),
        end: VttTimestamp::new(Duration::from_secs(5)),
        settings: None,
        payload: "Hello, world!".to_string(),
    };
    vtt.add_cue(cue);

    // Serialize the WebVtt instance
    let serialized = serde::ser::to_string(&vtt)?;
    println!("Serialized WebVtt:\n{}", serialized);

    Ok(())
}
```

**Deserializing a `WebVtt` instance:**

```rust
use vtt::prelude::*;
use serde::Deserialize;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let serialized_vtt = "\"WEBVTT
Language: en-US

1
00:00:01.000 --> 00:00:05.000
Hello, world!\"";

    // Deserialize the WebVtt instance
    let vtt: WebVtt = serde::de::from_str(&serialized_vtt)?;
    println!("Deserialized WebVtt:\n{}", vtt);

    Ok(())
}
```

**Note:** When serializing, the `WebVtt` instance is converted to its
WebVTT-formatted string and then serialized as a JSON string. Similarly, when
deserializing, the JSON string should contain the WebVTT-formatted text within
quotes.

## Documentation

Read the full documentation [here](https://docs.rs/vtt).

## Contributing

You can help make VTT better. Follow these steps:

1. Fork the repository.
2. Create a new branch.
3. Make your changes.
4. Submit a pull request.

Make sure your code follows the project's style and passes all tests.

## License

This project is licensed under the MIT or Apache-2.0 license.

## Contact

For questions or support, visit the
[repository](https://github.com/Govcraft/vtt).
## Sponsor

Govcraft is a one-person shop—no corporate backing, no investors, just me building useful tools. If this project helps you, [sponsoring](https://github.com/sponsors/Govcraft) keeps the work going.

[![Sponsor on GitHub](https://img.shields.io/badge/Sponsor-%E2%9D%A4-%23db61a2?logo=GitHub)](https://github.com/sponsors/Govcraft)
