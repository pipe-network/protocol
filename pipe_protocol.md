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
  "type": "message"   # string
}
```

## Profiles

### Data Structure

**Profile**

A profile contains information about the user.

Example:

```json
{
  "type": "profile",                            # string
  "first_name": "Hanky",                        # string
  "last_name": "Hank",                          # string
  "description": "I am Hanky Hank! :)"          # string
  "profile_picture": [0x12, 0xAF],              # byte array
  "profile_picture_mimetype": "image/png",      # string
  "public_key": "9611199a8ce7e2baad6c3a72..."   # string
}
```

### Messages

**Request Profile**

Request a profile to initialize/update a friend's profile.

Example:

```json
{
  "type": "request_profile",    # string
}
```

**Respond Profile**

Respond a profile to the requester

Example:

```json
{
  "type": "respond_profile",
  "profile": {
    "type": "profile",                            # string
    "first_name": "Hanky",                        # string
    "last_name": "Hank",                          # string
    "description": "I am Hanky Hank! :)"          # string
    "profile_picture": [0x12, 0xAF],              # byte array
    "profile_picture_mimetype": "image/png",      # string
    "public_key": "9611199a8ce7e2baad6c3a72..."   # string
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
  "id": "da721ce1-4841-442d-9167-45ab45d43528"    # uuid
  "timestamp": 1618661370,                        # big int
}
```

### Messages

**Request Feed Intros**

Request feed intros requests all feed intros available on the device

Example:

```json
{
  "type": "request_feed_intros",    # string
}
```

**Respond Feed Intros**

Respond feed intros responses all feed intros available on the device

```json
{
  "type": "respond_feed_intros",                      # string
  "feed_intros": [                                    # array<FeedIntro>
    {                                                 # FeedIntro
      "id": "da721ce1-4841-442d-9167-45ab45d43528"    # uuid
      "timestamp": 1618661370,                        # big int
    },
    ...
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
  "id": "da721ce1-4841-442d-9167-45ab45d43528"    # uuid
  "type": "BaseFeed",                             # string
  "timestamp": 1618661370,                        # big int
}
```

**TextFeed: BaseFeed**

A `TextFeed` inherits a BaseFeed and contains the additional field: `text`

Example:

```json
{
  "id": "da721ce1-4841-442d-9167-45ab45d43528"    # uuid
  "type": "TextFeed",                             # string
  "timestamp": 1618661370,                        # big int
  "text": "Some thoughts!",                       # string
}
```

**ImageFeed: TextFeed**

An `ImageField` inherits a TextFeed (because an Image has mostly a description) and contains the additional field: `image`

Example:

```json
{
  "id": "da721ce1-4841-442d-9167-45ab45d43528"    # uuid
  "type": "ImageFeed",                            # string
  "timestamp": 1618661370,                        # big int
  "text": "Some thoughts!",                       # string
  "image": [0x12, 0x43]                           # byte array
}
```

Responder requests feeds and initiators respond feeds:

### Messages

**Request feed**

Request feed requests a feed by it's uuid.

Example:

```json
{
  "type": "request_feed",                         # string
  "id": "da721ce1-4841-442d-9167-45ab45d43528"    # uuid
}
```

**Respond feed**

Respond feed responds a requests feed by it's id.

Example:

```json
{
  "type": "respond_feed"                            # string
  "feed": {                                         # feed
    "id": "da721ce1-4841-442d-9167-45ab45d43528"    # uuid
    "type": "ImageFeed",                            # string
    "timestamp": 1618661370,                        # big int
    "text": "Some thoughts!",                       # string
    "image": [0x12, 0x43]                           # byte array
  }
}
```
