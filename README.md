# BSOM_NOTES

this will cover five confidentiality modes to use with symmetric key block cipher algorithm.

* ECB(Electronic Codeblock)
* CBC(Cipher Block Chaining)
* CFB(Cipher Feedback)
* OFB(Output Feedback)
* CTR(Counter)

### Definitions, abbreviations, and symbols

**Definitions and Abbreviations**

* Bit
  - a binary digit: 0 or 1
* Bit Error
  - Substitution of a '0' bit for a '1' bit, or vice versa
* Bit String
  - ordered sequence of '0's and '1's
* Block Cipher
  - a set of functions and their inverse functions with a key parameter. The functions map bit strings of a fixed length to bit strings of the same length.
* Block Size
  - number of bits in an input or output block of the block cipher.
* CBC
  - Cipher Block Chaining Operation Mode.
* CFB
  - Cipher Feedback operation mode.
* Ciphertext
  - encrypted data
* Confidentiality Mode
  - mode used to encipher and decipher blocks(CBC, ECB, CFB, OFB, CTR)
* CTR
  - Counter operation mode.
* Cryptographic Key
  - parameter used in the block cipher algorithm that determined the forward cipher operation and the inverse cipher operation.
* Data Block
  - a sequence of bits with the block size length.
* Data Segment
  - a sequence of bits whose length is a parameter that does not exceed the block size.
* Decryption(deciphering)
  - the confidentiality mode that transforms encrypted data into original usable data.
* Encryption(enciphering)
  - the confidentiality mode that transforms plaintext into cipher version.
* ECB
  - electronic Codebook operation mode.
* Exclusive-OR
  - bitwise addition of bit strins of the same length.
* FIPS
  - Federal Information Processing Standard
* Forward Cipher Function(Forward Cipher Operation)
  - one of the two functions of the block cipher algorithm selected by the cryptographic key.
* Inverse Cipher Function(Inverse Cipher Operation)
  - the function that reverses the transformation of the forward cipher function if the key is equal.
* Initialization Vector(IV)
  - data block that some modes of operation require as initial input.
* Input Block
  - a data block that is an input to either a forward cipher function or the inverse cipher function of the block cipher algorithm.
* Least Significant Bit(s)
  - the right most bit(s) of a bit string.
* Most Significant Bit(s)
  - the left most bit(s) of a bit string.
* Mode of Operation(Mode)
  - an algorithm for a cryptographic transformation of data that uses a symmetric key block cipher algorithm.
* Nonce
  - a value that is used only once.
* Octet
  - a group of eight binary digits.
* OFB
  - Output Feedback operation mode.
* Output Block
  - a data block that is an output of either a forward cipher function or the inverse cipher function of the block cipher algorithm.
* Plaintext
  - usable data that is formatted as input to a mode.


## Underlying Block Cipher Algorithm

this documentation assumes that FIPS-approved symmetric key block cipher algorithms has been chosen as the underlying algorithm.
The cryprographyc key(K) regulates the functioning of the block cipher algorithm, meaning that it regulates the functioning of the mode.
A confidentiality mode of operation of the block cipher algorithm consists of two processes that are inverses of each other, encryption and decryption.
For any given Key(K), the block cipher algorithm of the mode also consists of two functions that are inverses of each other(encryption and decryption), but as
part of the choice of the block cipher algorithm one of the two functions is designed as *Forward Cipher Function*(CIPH), the other is the inverse cipher function, 
the input and output of both functions are called *input blocks* and *output blocks*, the input and the output block cipher algorithnm have the same bit length, called the block size(b).


### Representation of the Plaintext and Ciphertext

For *ECB* and *CBC* modes, the total number of bits in the plaintext must be a multiple of the block size(b), the plaintext is a sequence of n(bits), each with bit length b. The bit strings in the sequence are called data blocks, and the plaintext is denoted as P, P^n, P(n), etc.
For the *CFB* Mode, the total number of bits in the plaintext must be a multiple of a parameter(s), that does not exceed the block size. The bit strings in the sequence are called data segments.
For *OFB* and *CTR* Modes, the plaintext could not be a multiple of the block size. The plaintext consists of a sequence of n bit strings, in which the bit length of the last bit string is the number of bits in the last plaintext or ciphertext block, the bit strings are called data blocks.

For each mode, the encryption process transforms every plaintext data block or segment into a corresponding ciphertext data block or segment with the same bit length, so that the ciphertext is a sequence of data blocks or segments. 

### Initialization Vectors(IV)

the input to the encryption process of the CBC, CBF, OFB modes includes, in addition to the plaintext, a data block called the initialization vector(IV).
The IV is used in an initial step in the encryption of a message and in the corresponding decryption of the message.

The IV can also not be secret, but for CBC and CFB modes, the IV must be unpredictable for each execution, and for OFB mode, IV must be unique.

## Block Cipher Modes of Operation

### ECB(Electronic-Codebook) Mode

ECB mode is a way to encrypt data, it takes a piece of information called plaintext and turns it into a scrambled version called ciphertext using a specific key.
The process is done in blocks, this means that the plaintext is divided into smaller pieces (blocks) and each block is processed separately.
The formula is: *CJ* = *CIPHK(Pj)* for j = ... n. *Cj* is the ciphertext block for the j-th plaintext block.
*CIPH* is the encryption function that uses a key(K) to turn the plaintext block(Pj) into a ciphertext. This means that for each block of plaintext (P1, P2, ...Pn), you apply the encryption function to get the corresponding ciphertext(C1, C2, ..., Cn).

The Formula for decryption is: *Pj* - *CIPH -1(Cj)* for j = 1...n.
*Cj* is the ciphertext block, and *Pj* is the resulting plaintext block after description.
*CIPH -1* is the decryption function data reverses the encryption process. This means that for each block of ciphertext, you apply the decryption function to get back the original plaintext.
in *ECB* each block is processed independently, meaning that if two identical plaintext blocks are encrypted, they will produce the same cipher text block. This can be a security risk because patterns in the plaintext can be visible in the ciphertext.
*ECB* is simple and fast, but not the most secure method for encryption because of the way it handles identical blocks.
In *ECB* encryption and *ECB* decryption, multiple forward cipher functions and inverse cipher functions can be computed in parallel.
In *ECB* encryption and decryption data can processed in parallel. This means that if you have a plaintext block and encrypt it, it will always produce the same ciphertext everytime encrypted with that key(K), this can be bad for security.


### Cipher Block Chaining Mode(CBC)

The *CBC* mode combines each block of plaintext with the previous block of ciphertext before encrypting it. In *CBC* mode the plaintext is divided into blocks, but before the first block gets encrypted, an IV is used to start the process. For each subsequent block of plaintext, it takes the previous ciphertext block and combine it with the current block before encrypting. This *IV* can also not be secret, but it must be unpredictable for each encryption.

**CBC Encryption**
when execution starts, the first block of plaintext is combined with a random value(*IV*) using *XOR*, this creates the first input block.
Then encrypt this block to get the first ciphertext block.
For the second block of plaintext, it combines it with the first ciphertext block from the previous operation using XOR to create the second ciphertext block.
And so all the subsequent operations.

**CBC Decryption**
To decrypt the first ciphertext block, it applies the inverse of the encryption function and then XOR the result with the *IV* to get back the first plaintext block.
For the second ciphertext block, it applies the inverse function and then *XOR* the result with the first ciphertext block to recover the second plaintext block.
This process continues for all blocks, to recover any plaintext block(except the first), it applies the inverse function to the corresponding ciphertext block and XOR it with the previous ciphertext block.
*CBC* Encryption cannot happen in parallel because each block depends on the result of the previous block, but in decryption, it can process multiple ciphertext block in parallel since all ciphertext blocks are available at the same time.

### Cipher Feedback Mode(CFB)
