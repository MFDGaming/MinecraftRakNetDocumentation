# Data Types
| Type           | Length            | Notes                                                                                                                                                                                                                                                                                                                                       |
|----------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| unsigned char  | 1                 | A integer that ranges from 0 to 255                                                                                                                                                                                                                                                                                                         |
| char           | 1                 | A integer that ranges from -128 to 127                                                                                                                                                                                                                                                                                                      |
| unsigned short | 2                 | A integer that ranges from 0 to 65535                                                                                                                                                                                                                                                                                                       |
| short          | 2                 | A integer that ranges from -32768 to 32767                                                                                                                                                                                                                                                                                                  |
| unsigned triad | 3                 | A integer that ranges from 0 to 16777215                                                                                                                                                                                                                                                                                                    |
| triad          | 3                 | A integer that ranges from −8388608 to 8388607                                                                                                                                                                                                                                                                                              |
| unsigned int   | 4                 | A integer that ranges from 0 to 4294967295                                                                                                                                                                                                                                                                                                  |
| int            | 4                 | A integer that ranges from -2147483648 to 2147483647                                                                                                                                                                                                                                                                                        |
| unsigned long  | 8                 | A integer that ranges from 0 to 18446744073709551615                                                                                                                                                                                                                                                                                        |
| long           | 8                 | A integer that ranges from -9223372036854775808 to 9223372036854775807                                                                                                                                                                                                                                                                      |
| float          | 4                 | It's range is 3.4E +/- 38 (7 digits)                                                                                                                                                                                                                                                                                                        |
| double         | 8                 | It's range is 1.7E +/- 308 (15 digits)                                                                                                                                                                                                                                                                                                      |
| raknet magic   | 16                | The offline packet identifier of RakNet (00 ff ff 00 fe fe fe fe fd fd fd fd 12 34 56 78)                                                                                                                                                                                                                                                   |
| raknet string  | 2 + string length | A UTF-8 encoded string with a big endian unsigned short as a prefix to identify its length                                                                                                                                                                                                                                                  |
| raknet address | 7                 | The first byte is the address version (practically always 4). The next 4 bytes are the 4 parts of the IP address and they are decoded by subtracting the part's value from -1 and adding 255 to the result ((-1 - part) + 255). This applies for each byte one by one. The next 2 bytes are the port. They are a big endian unsigned short. |
| bool           | 1                 | Any number that isn't 0 is True. 0 will always be false.                                                                                                                                                                                                                                                                                    |

# Offline Packets

## Unconnected Ping (0x01)
| Field name       | Field Type     | Field Endianess |
|------------------|----------------|-----------------|
| id               | unsigned char  | N/A             |
| client timestamp | unsigned long  | big endian      |
| magic            | raknet magic   | N/A             |
| client guid      | unsigned long  | big endian      |

## Unconnected Ping Open Connections (0x02)
| Field name       | Field Type     | Field Endianess |
|------------------|----------------|-----------------|
| id               | unsigned char  | N/A             |
| client timestamp | unsigned long  | big endian      |
| magic            | raknet magic   | N/A             |
| client guid      | unsigned long  | big endian      |

The Unconnected Ping Open Connections is only sent when there are open
connections in the server.

## Unconnected Pong (0x1c)
| Field name       | Field Type     | Field Endianess |
|------------------|----------------|-----------------|
| id               | unsigned char  | N/A             |
| client timestamp | unsigned long  | big endian      |
| server guid      | unsigned long  | big endian      |
| magic            | raknet magic   | N/A             |
| data             | raknet string  | N/A             |

This is an example of what you can put in the data field.

```MCPE;Dedicated Server;440;1.17.0;0;10;13253860892328930865;Bedrock level;Survival;1;19132;19133;```

## Incompatible Protocol Version (0x19)
| Field name       | Field Type     | Field Endianess |
|------------------|----------------|-----------------|
| id               | unsigned char  | N/A             |
| protocol version | unsigned char  | N/A             |
| magic            | raknet magic   | N/A             |
| server guid      | unsigned long  | big endian      |

## Open Connection Request 1 (0x05)
| Field name       | Field Type     | Field Endianess |
|------------------|----------------|-----------------|
| id               | unsigned char  | N/A             |
| magic            | raknet magic   | N/A             |
| protocol version | unsigned char  | N/A             |
| mtu size         | char[]         | N/A             |

The actual mtu size is the size of the char array.
The protocol version in the latest version of Minecraft
Bedrock Edition is 10. If the protocol version of the
client is not the same as the server's the server responds
with a Incompatible Protocol Version packet

## Open Connection Reply 1 (0x06)
| Field name       | Field Type     | Field Endianess |
|------------------|----------------|-----------------|
| id               | unsigned char  | N/A             |
| magic            | raknet magic   | N/A             |
| server guid      | unsigned long  | big endian      |
| use security     | bool           | N/A             |
| mtu size         | unsigned short | big endian      |

## Open Connection Request 1 (0x07)
| Field name       | Field Type     | Field Endianess |
|------------------|----------------|-----------------|
| id               | unsigned char  | N/A             |
| magic            | raknet magic   | N/A             |
| server address   | raknet address | N/A             |
| mtu size         | unsigned short | big endian      |
| client guid      | unsigned long  | big endian      |

## Open Connection Reply 2 (0x08)
| Field name       | Field Type     | Field Endianess |
|------------------|----------------|-----------------|
| id               | unsigned char  | N/A             |
| magic            | raknet magic   | N/A             |
| server guid      | unsigned long  | big endian      |
| client address   | raknet address | N/A             |
| mtu size         | unsigned short | big endian      |
| use encryption   | bool           | N/A             |

# Acknowledements

## Records
A record can either be a single sequence number or a
range between 2 sequence numbers (if we have 1 and 8
the numbers will be [1, 2, 3, 4, 5, 6, 7, 8]).
### Record Packet Structure
| condition        | Field name            | Field Type      | Field Endianess |
|------------------|-----------------------|-----------------|-----------------|
| N/A              | is single             | bool            | N/A             |
| if is single     | sequence number       | unsigned triad  | little endian   |
| if is not single | sequence number start | unsigned triad  | little endian   |
| if is not single | sequence number end   | unsigned triad  | little endian   |

## Packet Structure
| Field name       | Field Type     | Field Endianess |
|------------------|----------------|-----------------|
| id               | unsigned char  | N/A             |
| record count     | unsigned short | big endian      |
| records          | record[]       | N/A             |

if the packet is used for sending the successfully arrived
packets the packet id is 0xc0. if the packet is used for
sending the not arrived packets the packet id is 0xa0.

# Frame Sets
## Reliability Type Table
| id   | name                              | is reliable | is sequenced | is ordered |
|------|-----------------------------------|-------------|--------------|------------|
| 0x00 | unreliable                        |             |              |            |  
| 0x01 | unreliable sequenced              |             | [ x ]            | x          |
| 0x02 | reliable                          | x           |              |            |
| 0x03 | reliable ordered                  | x           |              | x          |
| 0x04 | reliable sequenced                | x           | x            | x          |
| 0x05 | unreliable with ACK receipt       |             |              |            | 
| 0x06 | reliable with ACK receipt         | x           |              |            |
| 0x07 | reliable ordered with ACK receipt | x           |              | x          |

