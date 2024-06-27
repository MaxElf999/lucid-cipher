# lucid-cipher
A TypeScript library for simple *unsecure* (hence "lucid") ciphers.

This library is only intended to be used on English, ASCII-only plaintext strings.

## Ciphers

Every cipher has at least the following functions:

```typescript
// Encrypts the plaintext with the cipher using the given key.
cipherNameEncrypt(plaintext: string, key): string;

// Decrypts the ciphertext with the cipher using the given key.
cipherNameDecrypt(ciphertext: string, key): string;

// Attempts to break the cipher. Effectiveness varies by cipher.
cipherNameBreak(ciphertext: string): string;

// Attempts to brute force the cipher, checking each result with `test`.
cipherNameBrute(ciphertext: string, test: (t: string) => boolean): string;

// Generates every possible decryption of the cipher text.
cipherNameBruteGen(ciphertext: string): Generator<string>;
```
Any other functions pertaining to a cipher will be detailed at the end of its section below.

### Caesar Cipher | `caesar`

> [!CAUTION]
> **Not Implemented**

One of the simplest and well known ciphers is named after Julius Caesar who used it to encrypt military communications. The Caeser cipher works by substituting each letter in the plaintext with a letter a fixed number of letters away in the alphabet wrapping arround at the end. Caesar is attested to using a cipher with a rightward shift of 3 so "A" in the plaintext would become "D" in the ciphertext, "B" becomes "E", and "Z" wraps around to "C".

The full substitution for a right a shift of 3:
<table>
  <tbody>
    <tr>
      <th>Plain</th>
      <td><small>A</small></td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td><td>G</td><td>H</td><td>I</td><td>J</td><td>K</td><td>L</td><td>M</td><td>N</td><td>O</td><td>P</td><td>Q</td><td>R</td><td>S</td><td>T</td><td>U</td><td>V</td><td>W</td><td>X</td><td>Y</td><td>Z</td>
    </tr>
    <tr>
      <th>Cipher</th>
      <td>D</td><td>E</td><td>F</td><td>G</td><td>H</td><td>I</td><td>J</td><td>K</td><td>L</td><td>M</td><td>N</td><td>O</td><td>P</td><td>Q</td><td>R</td><td>S</td><td>T</td><td>U</td><td>V</td><td>W</td><td>X</td><td>Y</td><td>Z</td><td>A</td><td>B</td><td>C</td>
    </tr>
  </tbody>
</table>

Example encryption with a right a shift of 3:
```
Plaintext:  THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG
Ciphertext: WKH TXLFN EURZQ IRA MXPSV RYHU WKH ODCB GRJ
```

The number of letters to shift by serves as the key to the cipher. A positive number shifts rightward when encrypting and leftward when decrypting.

### Simple Substitution Cipher | `simpleSub`

> [!CAUTION]
> **Not Implemented**

A simple substitution cipher replaces every instance of each letter in a text with a different letter. The Caesar cipher above is a simple substitution cipher that doesn't change the order of the letters in the alphabet, but simple substitution ciphers can rearange the letters in any order.

Example substitution:
<table>
  <tbody>
    <tr>
      <th>Plain</th>
      <td>A</td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td><td>G</td><td>H</td><td>I</td><td>J</td><td>K</td><td>L</td><td>M</td><td>N</td><td>O</td><td>P</td><td>Q</td><td>R</td><td>S</td><td>T</td><td>U</td><td>V</td><td>W</td><td>X</td><td>Y</td><td>Z</td>
    </tr>
    <tr>
      <th>Cipher</th>
      <td>Z</td><td>R</td><td>E</td><td>L</td><td>O</td><td>H</td><td>W</td><td>P</td><td>U</td><td>N</td><td>V</td><td>D</td><td>T</td><td>M</td><td>Y</td><td>C</td><td>K</td><td>F</td><td>X</td><td>Q</td><td>I</td><td>J</td><td>S</td><td>A</td><td>B</td><td>G</td>
    </tr>
  </tbody>
</table>

Example encryption using the above substitution:
```
Plaintext:  THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG
Ciphertext: QPO KIUEV RFYSM HYA NITCX YJOF QPO DZGB LYW
```

The key is a string or array containg the shuffled alphabet used to substitute the letters. Any duplicate letters will be removed and any missing ones will be added to the end.

### Vigenère Cipher | `vigenere`

> [!CAUTION]
> **Not Implemented**

The Vigenère cipher was first described in 1553 by Giovan Battista Bellaso, but was much later misattributed to one of his contemporaries Blaise de Vigenère. It works by applying a different caesar shift to each letter in the plaintext based on the corresponding letter in the key (i.e. an "A" in the key means no shift, a "B" means a shift by 1, a "C" a shift by 2, and so on).

Vigenère square or tabula recta showing which letter to substitute for each combination of plaintext and key letters:

|       | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| **A** | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
| **B** | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A |
| **C** | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B |
| **D** | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C |
| **E** | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D |
| **F** | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E |
| **G** | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F |
| **H** | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G |
| **I** | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H |
| **J** | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I |
| **K** | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J |
| **L** | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K |
| **M** | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L |
| **N** | N | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L | M |
| **O** | O | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L | M | N |
| **P** | P | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O |
| **Q** | Q | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P |
| **R** | R | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q |
| **S** | S | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R |
| **T** | T | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S |
| **U** | U | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T |
| **V** | V | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U |
| **W** | W | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V |
| **X** | X | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W |
| **Y** | Y | Z | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X |
| **Z** | Z | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y |

Encription with the key "HELLOWORLD"
```
Plaintext:    THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG
Repeated Key: HEL LOWOR LDHEL LOW ORLDH ELLO WOR LDHE LLO
Ciphertext:   ALP BIEQB MUVAY QCT XLXSZ SGPF PVV WDGC OZU
```

The key can be any string but all non-alphabetical characters will be removed before it is used.
