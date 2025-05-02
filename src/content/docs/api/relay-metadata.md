---
title: Relay Metadata
description: .
---

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