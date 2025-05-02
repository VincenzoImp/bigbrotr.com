---
title: Event
description: The `Event` class represents a single NOSTR event.
---

The `Event` class represents a single NOSTR event.

## Attributes
- `id` (`str`): Unique identifier of the event, derived from event fields.
- `pubkey` (`str`): Public key of the event creator.
- `created_at` (`int`): Unix timestamp for when the event was created.
- `kind` (`int`): Integer representing the type of event.
- `tags` (`List[List[str]]`): List of tag lists (e.g., metadata).
- `content` (`str`): Text or JSON content of the event.
- `sig` (`str`): Cryptographic signature of the event.
- `content_obj` (`dict` or `None`): Parsed JSON if `content` is valid JSON, otherwise `None`.
---

## Methods

### `__init__`
Initialize an Event object.
#### Parameters
- `id` (`str`): ID of the event, must match the hash of the event data.
- `pubkey` (`str`): Public key of the event creator.
- `created_at` (`int`): Unix timestamp of when the event was created.
- `kind` (`int`): Type of event.
- `tags` (`List[List[str]]`): Tags associated with the event.
- `content` (`str`): Content string.
- `sig` (`str`): Cryptographic signature of the event.
#### Example
```python
>>> id = "0x123"
>>> pubkey = "0x123"
>>> created_at = 1612137600
>>> kind = 0
>>> tags = [["tag1", "tag2"]]
>>> content = "content"
>>> sig = "0x123"
>>> event = Event(id, pubkey, created_at, kind, tags, content, sig)
```
#### Returns
- `Event`: Event object initialized with the provided parameters.
#### Raises
- `TypeError`: If any input is of the wrong type.
- `ValueError`: If the event ID or signature is invalid.
---

### `__repr__`
Return a string representation of the Event object.
#### Example
```python
>>> event = Event(id, pubkey, created_at, kind, tags, content, sig)
>>> event
Event(id=0x123, pubkey=0x123, created_at=1612137600, kind=0, tags=[["tag1", "tag2"]], content=content, sig=0x123)
```
#### Returns
- `str`: String representation of the Event object.
---

### `from_dict`
Create an Event object from a dictionary.
#### Parameters
- `data` (`dict`): Dictionary with keys `"id"`, `"pubkey"`, `"created_at"`, `"kind"`, `"tags"`, `"content"`, and `"sig"`.
#### Example
```python
>>> data = {"id": "0x123", "pubkey": "0x123", "created_at": 1612137600, "kind": 0, "tags": [["tag1", "tag2"]], "content": "content", "sig": "0x123"}
>>> event = Event.from_dict(data)
Event(id=0x123, pubkey=0x123, created_at=1612137600, kind=0, tags=[["tag1", "tag2"]], content=content, sig=0x123)
```
#### Returns
- `Event`: Event object created from the dictionary.
#### Raises
- `TypeError`: If `data` is not a dictionary.
- `KeyError`: If data does not contain the required keys.
---

### `to_dict`
Return a dictionary representation of the Event object.
#### Example
```python
>>> event = Event(id, pubkey, created_at, kind, tags, content, sig)
>>> event.to_dict()
{'id': '0x123', 'pubkey': '0x123', 'created_at': 1612137600, 'kind': 0, 'tags': [['tag1', 'tag2']], 'content': 'content', 'sig': '0x123', 'content_obj': None}
```
#### Returns
- `dict`: Dictionary representation of the Event object.
---