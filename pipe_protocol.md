# Pipe Protocol

All of the messages underneath can only be shared if a secure connection was established and initiator and responder have shared their public keys in their friend lists.

## Message serialization

All messages are written here in json format.
Nevertheless all messages should be serialized with [MessagePack](https://msgpack.org/).

## Common

The common section provides common data structure for all messages.

### Data Structure

**Messages and Data Structures**

A message and a data structure always consists of a `type` field which indicicates the type of it's kind.
Every type is unique and is always expressed as a string.

Example:

```json
{
  "type": "Message"
}
```

## Profiles

### Data Structure

**Profile**

A profile contains information about the user.

Example:

```json
{
  "type": "Profile",
  "first_name": "Hanky",
  "last_name": "Hank",
  "description": "I am Hanky Hank! :)",
  "profile_picture": ["0x12", "0xAF"],
  "profile_picture_mimetype": "image/png",
  "public_key": "9611199a8ce7e2baad6c3a72..."
}
```

### Messages

**Request Profile**

Request a profile to initialize/update a friend's profile.

Example:

```json
{
  "type": "request_profile"
}
```

**Respond Profile**

Respond a profile to the requester

Example:

```json
{
  "type": "respond_profile",
  "profile": {
    "type": "Profile",
    "first_name": "Hanky",
    "last_name": "Hank",
    "description": "I am Hanky Hank! :)",
    "profile_picture": ["0x12", "0xAF"],
    "profile_picture_mimetype": "image/png",
    "public_key": "9611199a8ce7e2baad6c3a72..."
  },
}
```

**Respond Profile**

## FeedIntros

### Data Structure

**FeedIntro**

A FeedIntro is just a part of a feed, to transport less data over the network.
Actually it just contains the id and timestamp of a feed.

Example:

```json
{
  "id": "da721ce1-4841-442d-9167-45ab45d43528",
  "timestamp": 1618661370
}
```

### Messages

**Request Feed Intros**

Request feed intros requests all feed intros available on the device

Example:

```json
{
  "type": "request_feed_intros"
}
```

**Respond Feed Intros**

Respond feed intros responses all feed intros available on the device

```json
{
  "type": "respond_feed_intros",
  "feed_intros": [
    {
      "id": "da721ce1-4841-442d-9167-45ab45d43528",
      "timestamp": 1618661370
    }
  ]
}
```

## Feeds

Feeds contain Texts or Images and are shown in the Feed Pipeline

### Data Structure

There are several types of feeds

- BaseFeed
- TextFeed
- ImageFeed

**BaseFeed**

A `BaseFeed` is an abstract feed, which won't be used in messages.
It has just some basic fields, which will be inherited from other `Feeds`.

Example:

```json
{
  "id": "da721ce1-4841-442d-9167-45ab45d43528",
  "type": "BaseFeed",
  "timestamp": 1618661370
}
```

**TextFeed: BaseFeed**

A `TextFeed` inherits a BaseFeed and contains the additional field: `text`

Example:

```json
{
  "id": "da721ce1-4841-442d-9167-45ab45d43528",
  "type": "TextFeed",
  "timestamp": 1618661370,
  "text": "Some thoughts!"
}
```

**ImageFeed: TextFeed**

An `ImageField` inherits a TextFeed (because an Image has mostly a description) and contains the additional field: `image`

Example:

```json
{
  "id": "da721ce1-4841-442d-9167-45ab45d43528",
  "type": "ImageFeed",
  "timestamp": 1618661370,
  "text": "Some thoughts!",
  "image": ["0x12", "0x43"]
}
```

Responder requests feeds and initiators respond feeds:

### Messages

**Request feed**

Request feed requests a feed by it's uuid.

Example:

```json
{
  "type": "request_feed",
  "id": "da721ce1-4841-442d-9167-45ab45d43528"
}
```

**Respond feed**

Respond feed responds a requests feed by it's id.

Example:

```json
{
  "type": "respond_feed",
  "feed": {
    "id": "da721ce1-4841-442d-9167-45ab45d43528",
    "type": "ImageFeed",
    "timestamp": 1618661370,
    "text": "Some thoughts!",
    "image": ["0x12", "0x43"]
  }
}
```
