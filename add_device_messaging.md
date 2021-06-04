# Add Device Token Messaging

This is document describes the messaging between a Client and a Server to register a device token with the public key for Push Notifications.

## Sender

## Receiver

## Message

An "Add Device Token" Message is always build in the following example:

```
+---------+--------------------------+----------------+
| Header  | 32 bytes public key      | 24 bytes nonce |
+---------+--------------------------+----------------+
| Message | x bytes AddDeviceMessage | ...            |
+---------+--------------------------+----------------+
```

**Start of Header**

- (0-31 byte) The Public key of the sender
- (32-56) Random initiated nonce

**End of Header**

**Start of actual message**

- (57-x) the add device message itself

**End of actual message**

## AddDeviceRequestMessage

The `Client` sends an `AddDeviceRequestMessage` to the `Server` to notify about a new device token.
The `AddDeviceControlMessage` is **not** encrypted via NaCl library.

Example:

```json
{
  "type": "add_device_request_message"
}
```

## AddDeviceControlMessage

The `Server` responds to an `AddDeviceRequestMessage` with an `AddDeviceControlMessage`.
The message contains a randomly generated uuid.
The `AddDeviceControlMessage` is encrypted via NaCl library.

Example:

```json
{
  "type": "add_device_control_message",
  "uuid": "49cc7e08-1d9c-42d8-af0c-2c6118f78efc"
}
```

## AddDeviceSolvedMessage

The `Client` responds to an `AddDeviceControlMessage` with an `AddDeviceSolvedMessage`.
The message contains the randomly generated uuid from the previous `AddDeviceControlMessage` and the new device token.
The `AddDeviceSolvedMessage` is encrypted via NaCl library.

Example:

```json
{
  "type": "add_device_solved_message",
  "uuid": "49cc7e08-1d9c-42d8-af0c-2c6118f78efc",
  "device_token": "..."
}
```

The server should check if the uuid is the same as delivered in the `AddDeviceControlMessage`. If it's the same, then store the device token together with the public key.
