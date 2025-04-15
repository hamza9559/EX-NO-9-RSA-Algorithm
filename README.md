# EX-NO-9-RSA-Algorithm

## AIM:
To Implement RSA Encryption Algorithm in Cryptography

## Algorithm:


Step 1: Design of RSA Algorithm  
The RSA algorithm is based on the mathematical difficulty of factoring the product of two large prime numbers. It involves generating a public and private key pair, where the public key is used for encryption, and the private key is used for decryption.

Step 2: Implementation in Python or C 
This algorithm can be implemented in languages like Python or C by performing large integer calculations for key generation, encryption, and decryption, utilizing libraries for modular arithmetic if necessary.

Step 3: Algorithm Description  
1. Key Generation:
   - Select two large prime numbers \( p \) and \( q \).
   - Calculate \( n = p \times q \), which will be used as the modulus.
   - Compute the totient \( \phi(n) = (p - 1)(q - 1) \).
   - Choose a public exponent \( e \) such that \( e \) is coprime with \( \phi(n) \).
   - Compute the private key \( d \), which is the modular inverse of \( e \) mod \( \phi(n) \).

2. Encryption:
   - Convert the plaintext message \( M \) into a numerical form \( m \) (such that \( 0 \le m < n \)).
   - Compute the ciphertext \( c \) using the formula: \( c = m^e \mod n \).

3. Decryption:
   - Use the private key \( d \) to recover \( m \) from \( c \) using: \( m = c^d \mod n \).
   - Convert \( m \) back into the original message \( M \).

Step 4: Mathematical Representation  
- Encryption: \( E(m) = m^e \mod n \)
- Decryption: \( D(c) = c^d \mod n \)

Step 5: **Security Foundation  
The security of RSA relies on the difficulty of factoring large numbers; thus, choosing sufficiently large prime numbers for \( p \) and \( q \) is crucial for security.

## Program:
```c
#include <stdio.h>
#include <string.h>

#define MAX 100

int gcd(int a, int b) {
    while (b != 0) {
        int t = b;
        b = a % b;
        a = t;
    }
    return a;
}

int modInverse(int e, int phi) {
    int t = 0, newt = 1;
    int r = phi, newr = e;

    while (newr != 0) {
        int quotient = r / newr;
        int temp = newt;
        newt = t - quotient * newt;
        t = temp;

        temp = newr;
        newr = r - quotient * newr;
        r = temp;
    }

    if (r > 1) return -1;
    if (t < 0) t += phi;
    return t;
}

long long power(long long base, long long exp, long long mod) {
    long long result = 1;
    base %= mod;
    while (exp > 0) {
        if (exp % 2 == 1)
            result = (result * base) % mod;
        exp >>= 1;
        base = (base * base) % mod;
    }
    return result;
}

int main() {
    int p = 11, q = 13;
    int n = p * q;
    int phi = (p - 1) * (q - 1);
    int e = 7;

    while (gcd(e, phi) != 1) e++;

    int d = modInverse(e, phi);
    if (d == -1) {
        printf("Modular inverse for e doesn't exist.\n");
        return 1;
    }

    char msg[MAX];
    printf("Enter the message to encrypt: ");
    fgets(msg, MAX, stdin);
    msg[strcspn(msg, "\n")] = '\0';

    int len = strlen(msg);
    long long encrypted[MAX], decrypted[MAX];
    char final[MAX];

    // Encrypt each character
    printf("\nEncrypted ASCII values: ");
    for (int i = 0; i < len; i++) {
        encrypted[i] = power((int)msg[i], e, n);
        printf("%lld ", encrypted[i]);
    }

    // Decrypt each character
    for (int i = 0; i < len; i++) {
        decrypted[i] = power(encrypted[i], d, n);
        final[i] = (char)decrypted[i];
    }
    final[len] = '\0';

    printf("\nDecrypted message: %s\n", final);

    printf("\nPublic Key: (%d, %d)", e, n);
    printf("\nPrivate Key: (%d, %d)\n", d, n);

    return 0;
}

```
## Output:
![image](https://github.com/user-attachments/assets/4b76dd89-3816-4c5d-a9e6-fac99fe16252)

## Result:
 The program is executed successfully.
