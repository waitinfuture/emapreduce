# Binary functions {#concept_gxl_m1h_ygb .concept}

## Concatenation operator {#section_rs3_q1h_ygb .section}

The `||` operator performs binary concatenation.

## Binary functions { .section}

|Function|Syntax|Description|
|--------|------|-----------|
|length|length\(binary\) → bigint|Returns the length of a binary block in bytes.|
|concat|concat\(binary1, ..., binaryN\) → varbinary|Returns the concatenation of binary1, binary2, …, binaryN.|
|to\_base64|to\_base64\(binary\) → varchar|Encodes binary data into a Base64 string representation.|
|from\_base64|from\_base64\(string\) → varbinary|Base64 decoding|
|to\_base64url|to\_base64url\(binary\) → varchar|Encodes binary into a Base64 string representation by using the URL safe alphabet.|
|from\_base64url|from\_base64url\(string\) → varbinary|Decodes binary data from a Base64-encoded string by using the URL safe alphabet.|
|to\_hex|to\_hex\(binary\) → varchar|Encodes binary data into a hex string representation.|
|from\_hex|from\_hex\(string\) → varbinary|Decodes binary data from a hex encoded string.|
|to\_big\_endian\_64|to\_big\_endian\_64\(bigint\) → varbinary|Encodes bigint values into a 64-bit two's complement big endian format.|
|from\_big\_endian\_64|from\_big\_endian\_64\(binary\) → bigint|Decodes bigint values from a 64-bit two's complement big endian binary.|
|to\_ieee754\_32|to\_ieee754\_32\(real\) → varbinary|Encodes real in a 32-bit big endian binary in the [IEEE 754](http://grouper.ieee.org/groups/754/) single-precision floating-point format.|
|to\_ieee754\_64|to\_ieee754\_64\(double\) → varbinary|Encodes double in a 64-bit big endian binary in the [IEEE 754](http://grouper.ieee.org/groups/754/) double-precision floating-point format.|
|crc32|crc32\(binary\) → bigint|Computes the CRC-32 of binary.|
|md5|md5\(binary\) → varbinary|Computes the MD5 hash of binary.|
|sha1|sha1\(binary\) → varbinary|Computes the SHA-1 hash of binary.|
|sha256|sha256\(binary\) → varbinary|Computes the SHA-256 hash of binary.|
|sha512|sha512\(binary\) → varbinary|Computes the SHA-512 hash of binary.|
|xxhash64|xxhash64\(binary\) → varbinary|Computes the xxHash 64 hash of binary.|

