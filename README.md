The ContainerCreator is a java library and commandline tool to build and unpack containers that utilize ABE for file encryption. It supports expressive monotonic boolean formulas for BSW07 (CPABE) and LW14 (TRABE). It can pack multiple files into one container.

# Build package

```
$ mvn package
```	

or without running tests:
	
```
$ mvn package -DskipTests
```

This will generate a fat jar in addition to the jars that contain this library, its source and its javadocs in the `target` directory.

# Simple commandline interface

The fat jar under `target/containerCreator-$(version)-jar-with-dependencies.jar` can be used to encrypt and decrypt files in a simple fashion (without expiration).

Run it without any arguments to see the usage guidance:

```
$ cd target
$ java -jar containerCreator-$(version)-jar-with-dependencies.jar
```

# TODOs

- Investigate and move the file format definition to [Nail: A practical tool for parsing and generating data formats](https://people.csail.mit.edu/nickolai/papers/bangert-nail.pdf) for easier and more robust implementation

- Some information such as file name is currently stored in plain text in the container manifest. This information should probably not be stored in plain text, so it should be encrypted along with the plaintext and only revealed once correctly decrypted.

- Currently, every file is encrypted on its own, even if there are other files with exactly the same policy and other properties. Since ABE is extremely slow compared to symmetric ciphers such as AES, it is better to AES-encrypt multiple files with the same ABE header. This would be called "FileBag". The same symmetric key can be used for every file, but the IV/nonce must be different.

- Add message authentication code over every single ciphertext separately to detect tampering/bit-flips. Encrypt-then-MAC is recommended with at least HMAC-SHA256. The file format must be extended to house the authentication tag (also called MAC).
