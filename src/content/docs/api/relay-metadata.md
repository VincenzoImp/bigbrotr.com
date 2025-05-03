---
title: Relay Metadata
description: The `RelayMetadata` class represents the metadata of a NOSTR relay server.
---

The `RelayMetadata` class represents the metadata of a NOSTR relay server.

## Attributes
| Name                 | Type                       | Description                                         |
| -------------------- | -------------------------- | --------------------------------------------------- |
| `relay_url`          | `str`                      | The URL of the relay.                               |
| `relay_network`      | `str`                      | The network of the relay (`clearnet`, `tor`, etc.). |
| `generated_at`       | `int`                      | Timestamp when the metadata was generated.          |
| `connection_success` | `bool`                     | Whether the connection to the relay was successful. |
| `nip11_success`      | `bool`                     | Whether NIP-11 metadata was successfully retrieved. |
| `readable`           | `Optional[bool]`           | Whether the relay supports reading.                 |
| `writable`           | `Optional[bool]`           | Whether the relay supports writing.                 |
| `rtt`                | `Optional[int]`            | Round-trip time for the connection in milliseconds. |
| `name`               | `Optional[str]`            | Name of the relay.                                  |
| `description`        | `Optional[str]`            | Description of the relay.                           |
| `banner`             | `Optional[str]`            | URL to the banner image.                            |
| `icon`               | `Optional[str]`            | URL to the icon image.                              |
| `pubkey`             | `Optional[str]`            | Public key of the relay operator.                   |
| `contact`            | `Optional[str]`            | Contact information.                                |
| `supported_nips`     | `Optional[List[int]]`      | List of supported NIPs.                             |
| `software`           | `Optional[str]`            | Name of the software the relay is running.          |
| `version`            | `Optional[str]`            | Version of the relay software.                      |
| `privacy_policy`     | `Optional[str]`            | URL to the privacy policy.                          |
| `terms_of_service`   | `Optional[str]`            | URL to the terms of service.                        |
| `limitations`        | `Optional[Dict[str, Any]]` | Dictionary of relay limitations.                    |
| `extra_fields`       | `Optional[Dict[str, Any]]` | Additional custom fields.                           |

## Methods

### `__init__`
Initialize a RelayMetadata object.
#### Parameters
- `relay`: `Relay`, the relay object.
- `generated_at`: `int`, the timestamp when the metadata was generated.
- `connection_success`: `bool`, indicates if the connection to the relay was successful.
- `nip11_success`: `bool`, indicates if the NIP-11 metadata was successfully retrieved.
- `readable`: `Optional[bool]`, indicates if the relay is readable.
- `writable`: `Optional[bool]`, indicates if the relay is writable.
- `rtt`: `Optional[int]`, the round-trip time for the connection.
- `name`: `Optional[str]`, the name of the relay.
- `description`: `Optional[str]`, a description of the relay.
- `banner`: `Optional[str]`, the URL of the banner image.
- `icon`: `Optional[str]`, the URL of the icon image.
- `pubkey`: `Optional[str]`, the public key of the relay.
- `contact`: `Optional[str]`, the contact information for the relay.
- `supported_nips`: `Optional[List[int]]`, a list of supported NIPs.
- `software`: `Optional[str]`, the software used by the relay.
- `version`: `Optional[str]`, the version of the software.
- `privacy_policy`: `Optional[str]`, the URL of the privacy policy.
- `terms_of_service`: `Optional[str]`, the URL of the terms of service.
- `limitations`: `Optional[Dict[str, Any]]`, limitations of the relay.
- `extra_fields`: `Optional[Dict[str, Any]]`, additional fields for custom metadata.
#### Example
```python
>>> relay = Relay("wss://relay.example.com")
>>> relay_metadata = RelayMetadata(
        relay=relay,
        generated_at=1612137600,
        connection_success=True,
        nip11_success=True,
        readable=True,
        writable=True,
        rtt=100,
        name="Example Relay",
        description="An example NOSTR relay",
        banner="https://example.com/banner.png",
        icon="https://example.com/icon.png",
        pubkey="abcdef1234567890",
        contact="",
        supported_nips=[1, 2, 3],
        software="nostr-relay-software",
        version="1.0.0",
        privacy_policy="https://example.com/privacy",
        terms_of_service="https://example.com/terms",
        limitations={"max_connections": 100},
        extra_fields={"custom_field": "value"}
    )
```
#### Returns
- `RelayMetadata`: RelayMetadata object initialized with the provided parameters.
#### Raises
- `TypeError`: If any input is of the wrong type.
- `TypeError`: If `limitations` keys are not strings.
- `TypeError`: If `extra_fields` keys are not strings.
---

### `__repr__`
Return a string representation of the RelayMetadata object.
#### Example
```python
>>> relay_metadata = RelayMetadata(
        relay_url="wss://relay.example.com",
        generated_at=1612137600,
        connection_success=False,
        nip11_success=False
    )
>>> print(relay_metadata)
RelayMetadata(relay_url=wss://relay.example.com, generated_at=1612137600, connection_success=False, nip11_success=False)
```
#### Returns
- `str`: String representation of the RelayMetadata object.
#### Raises
- `None`
---

### `from_dict`
Creates a RelayMetadata object from a dictionary.
#### Parameters
- `data`: `dict`, dictionary representation of the RelayMetadata object.
#### Example
```python
data = {
    "relay_url": "wss://relay.example.com",
    "generated_at": 1612137600,
    "connection_success": False,
    "nip11_success": False
}
relay_metadata = RelayMetadata.from_dict(data)
RelayMetadata(relay_url=wss://relay.example.com, generated_at=1612137600, connection_success=False, nip11_success=False)
```
#### Returns
- `RelayMetadata`: RelayMetadata object created from the dictionary.
#### Raises
- `TypeError`: If `data` is not a dict.
- `KeyError`: If `data` does not contain the required keys.
---

### `to_dict`
Return a dictionary representation of the RelayMetadata object.
#### Example
```python
relay_metadata = RelayMetadata(
    relay_url="wss://relay.example.com",
    generated_at=1612137600,
    connection_success=False,
    nip11_success=False
)
print(relay_metadata.to_dict())
{'relay_url': 'wss://relay.example.com', 'generated_at': 1612137600, 'connection_success': False, 'nip11_success': False}
```
#### Returns
- `dict`: Dictionary representation of the RelayMetadata object.
#### Raises
- `None`
---