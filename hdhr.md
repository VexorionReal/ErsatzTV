The discover protocol (UDP port 65001) and control protocol (TCP port 65001)
both use the same packet based format:
	uint16_t	Packet type
	uint16_t	Payload length (bytes)
	uint8_t[]	Payload data (0-n bytes).
	uint32_t	CRC (Ethernet style 32-bit CRC)

All variables are big-endian except for the crc which is little-endian.

Valid values for the packet type are listed below as defines prefixed
with "HDHOMERUN_TYPE_"

Discovery:

The payload for a discovery request or reply is a simple sequence of
tag-length-value data:
	uint8_t		Tag
	varlen		Length
	uint8_t[]	Value (0-n bytes)

The length field can be one or two bytes long.
For a length <= 127 bytes the length is expressed as a single byte. The
most-significant-bit is clear indicating a single-byte length.
For a length >= 128 bytes the length is expressed as a sequence of two bytes as follows:
The first byte is contains the least-significant 7-bits of the length. The
most-significant bit is then set (add 0x80) to indicate that it is a two byte length.
The second byte contains the length shifted down 7 bits.

A discovery request packet has a packet type of HDHOMERUN_TYPE_DISCOVER_REQ and should
contain two tags: HDHOMERUN_TAG_DEVICE_TYPE and HDHOMERUN_TAG_DEVICE_ID.
The HDHOMERUN_TAG_DEVICE_TYPE value should be set to HDHOMERUN_DEVICE_TYPE_TUNER.
The HDHOMERUN_TAG_DEVICE_ID value should be set to HDHOMERUN_DEVICE_ID_WILDCARD to match
all devices, or to the 32-bit device id number to match a single device.

The discovery response packet has a packet type of HDHOMERUN_TYPE_DISCOVER_RPY and has the
same format as the discovery request packet with the two tags: HDHOMERUN_TAG_DEVICE_TYPE and
HDHOMERUN_TAG_DEVICE_ID. In the future additional tags may also be returned - unknown tags
should be skipped and not treated as an error.

#define HDHOMERUN_DISCOVER_UDP_PORT 65001
#define HDHOMERUN_CONTROL_TCP_PORT 65001

#define HDHOMERUN_MAX_PACKET_SIZE 1460
#define HDHOMERUN_MAX_PAYLOAD_SIZE 1452

#define HDHOMERUN_TYPE_DISCOVER_REQ 0x0002
#define HDHOMERUN_TYPE_DISCOVER_RPY 0x0003
#define HDHOMERUN_TYPE_GETSET_REQ 0x0004
#define HDHOMERUN_TYPE_GETSET_RPY 0x0005
#define HDHOMERUN_TYPE_UPGRADE_REQ 0x0006
#define HDHOMERUN_TYPE_UPGRADE_RPY 0x0007

#define HDHOMERUN_TAG_DEVICE_TYPE 0x01
#define HDHOMERUN_TAG_DEVICE_ID 0x02
#define HDHOMERUN_TAG_GETSET_NAME 0x03
#define HDHOMERUN_TAG_GETSET_VALUE 0x04
#define HDHOMERUN_TAG_GETSET_LOCKKEY 0x15
#define HDHOMERUN_TAG_ERROR_MESSAGE 0x05
#define HDHOMERUN_TAG_TUNER_COUNT 0x10
#define HDHOMERUN_TAG_LINEUP_URL 0x27
#define HDHOMERUN_TAG_STORAGE_URL 0x28
#define HDHOMERUN_TAG_DEVICE_AUTH_BIN_DEPRECATED 0x29
#define HDHOMERUN_TAG_BASE_URL 0x2A
#define HDHOMERUN_TAG_DEVICE_AUTH_STR 0x2B
#define HDHOMERUN_TAG_STORAGE_ID 0x2C

#define HDHOMERUN_DEVICE_TYPE_WILDCARD 0xFFFFFFFF
#define HDHOMERUN_DEVICE_TYPE_TUNER 0x00000001
#define HDHOMERUN_DEVICE_TYPE_STORAGE 0x00000005
#define HDHOMERUN_DEVICE_ID_WILDCARD 0xFFFFFFFF

#define HDHOMERUN_MIN_PEEK_LENGTH 4

