# protocol

The pipe network protocol

## Signaling Protocol based on [SaltyRTC](https://github.com/saltyrtc/saltyrtc-meta)

This is roughly a description how a "PipeConnection" (it's actually a SaltyRTC connection but with a fixed flow) should be set up.

![Signaling Protocol](https://raw.githubusercontent.com/pipe-network/protocol/main/out/signaling_protocol/Signaling%20Protocol.png)

## Multiple Connections [DEPRECATED]

Following sequence diagram shows, what to do if multiple reponders try to connect to an initiator.

> Note: Do not use this anymore. Just try to connect to initiator, if the responder gets rejected, try maximum amount of 3 times and then circuit brake.

![Multiple Connection](https://raw.githubusercontent.com/pipe-network/protocol/main/out/multiple_connections/Multiple%20Connection%20Protocol.png)

## Pipe Messaging Protocol

Following sequence diagram shows how

![Pipe Messaging Protocol](https://raw.githubusercontent.com/pipe-network/protocol/main/out/pipe_messaging/Pipe%20Messaging%20Protocol.png)

## Add device token

This sequence diagram shows the communication between the signaling_server and a device to update the signaling_server with the new device token of the device.

![Add device token](https://raw.githubusercontent.com/pipe-network/protocol/main/out/add_device_token/Add%20device%20token.png)
