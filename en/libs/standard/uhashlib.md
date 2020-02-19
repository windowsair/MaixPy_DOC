uhashlib – hashing algorithms
====


This module implements a subset of the corresponding [CPython](http://docs.micropython.org/en/latest/reference/glossary.html#term-cpython) module, as described below. For more information, refer to the original CPython documentation: [hashlib](https://docs.python.org/3.5/library/hashlib.html#module-hashlib).

This module implements binary data hashing algorithms. The exact inventory of available algorithms depends on a board. Among the algorithms which may be implemented:

* SHA256 - The current generation, modern hashing algorithm (of SHA2 series). It is suitable for cryptographically-secure purposes. Included in the MicroPython core and any board is recommended to provide this, unless it has particular code size constraints.

Hardware accelerate is enabled on K210.

## Constructors

## class uhashlib.sha256([data])

Create an SHA256 hasher object and optionally feed data into it.


## Methods

### hash.update(data)

Feed more binary data into hash.

### hash.digest()

Return hash for all data passed through hash, as a bytes object. After this method is called, more data cannot be fed into the hash any longer.

### hash.hexdigest()

This method is NOT implemented. Use `ubinascii.hexlify(hash.digest())` to achieve a similar effect.

