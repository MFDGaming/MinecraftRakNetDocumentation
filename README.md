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

# Packets

## Unconnected Ping (0x01)
| Field name       | Field Type    | Field Endianess |
|------------------|---------------|-----------------|
| id               | unsigned byte | N/A             |
| client timestamp | unsigned long | big endian      |
| magic            | raknet magic  | N/A             |
| client guid      | unsigned long | big endian      |

## Unconnected Ping Open Connections (0x02)
| Field name       | Field Type    | Field Endianess |
|------------------|---------------|-----------------|
| id               | unsigned byte | N/A             |
| client timestamp | unsigned long | big endian      |
| magic            | raknet magic  | N/A             |
| client guid      | unsigned long | big endian      |

The Unconnected Ping Open Connections is only sent when there are open
connections in the server.

## Unconnected Pong (0x1c)
| Field name       | Field Type    | Field Endianess |
|------------------|---------------|-----------------|
| id               | unsigned byte | N/A             |
| client timestamp | unsigned long | big endian      |
| server guid      | unsigned long | big endian      |
| magic            | raknet magic  | N/A             |
| data             | raknet string | N/A             |

This is an example of what you can put in the data field.
`MCPE;Dedicated Server;440;1.17.0;0;10;13253860892328930865;Bedrock level;Survival;1;19132;19133;`
