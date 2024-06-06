We deconstruct objects or data structures in a format that enables easy storage/transfer and reconstruct them whenever required. This is serialization.

Ruby provides two different mechanisms to send data to another program across the web or save Ruby objects in a files:
1. Binary
2. Human -Readable
	- YAML
	- JSOn

## 1. Binary Format

Ruby supports binary serialization through the `Marshal` module [available in its standard library](https://ruby-doc.org/core-2.6.3/Marshal.html).
The marshalling library transforms the collection of Ruby objects into a stream of bytes that we humans can't decipher but Ruby can. The `Marshal.dump` method is used to convert the object to a byte stream and the `Marshal.load or Marshal.restore` method reconstructs the object.





