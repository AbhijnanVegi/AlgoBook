# How does encryption work ?

**Encryption** is the processes of encoding information. This process converts the original representation of information into an alternative form caller cipher text. Encryption protects that data you send via the internet like messages, mails, etc. Encryption allows an authorized party to decrypt the data and prevents the data from being visible to any prying eyes. It also prevents reading your data from being read without authorization.

An encryption scheme usually uses a pseudo-random encryption key generated by an algorithm. This key will be used to decrypt the data encrypted by the algorithm. It is possible to decrypt the message without possessing the key but, for a good algorithm considerable amount of computational resources and skills are required. A user is authorized by sharing this key with them.

There a two types of cryptographic systems

1. Symmetric key, where both encryption and decryption keys are the same
2. Asymmetric or Public Key, where encryption key is published for everyone to use, however only the party with decryption key can read it.

We will now describe the **RSA** algorithm, a very popular public key cryptosystem.

## RSA

**RSA** is short for Rivest-Shamir-Adleman, the surnames of the people who publicly described the algorithm. In a public-key cryptosystem, the encryption key used to encrypt the data is public and is different from the decryption key used to decrypt the data which is kept a secret.

The RSA algorithm creates a public and private key pair based on two large prime numbers, which are kept secret. Messages can be encrypted by anyone by anyone with the public key but can only be decrypted using the private key. Authorizing a party is as simple as sharing the private key with it.

The security of RSA relies on the practical difficulty of factoring the product of two large prime numbers. There are no published methods to decrypt the message in reasonable time if a large key is used. RSA is a relatively slow algorithm, because of this it is not commonly used to directly encrypt user data.

RSA involves four steps:

1. Key generation
2. Key distribution
3. Encryption
4. Decryption

**Principle** : The basic principle behind RSA come from the observation that it is practical to find three very large positive integers $e,d$ and $n$, such that with modular exponentiation for all integers $m$, the below equation is satisfied
$$
(m^e)^d \equiv m\ (mod\ n)
$$
and the fact that even after knowing $e,n$ and $m$, it can be extremely difficult to find $d$. The public key is represented by integer $n$ and $e$, and the private key by the integer $d$. $m$ represents the message and the method for generating it is described below.

1. **Key generation**

   1. Choose two distinct prime numbers $p$ and $q$

      - For security purposes they are chosen at random and should be similar in magnitude but differ in length by a few digits to make factoring harder.
      - Prime numbers can be efficiently found using the primality test, where you check whether the random number generated is prime or not in polynomial time. A simple primality test is trial division where you check the divisibility of n with prime numbers between 2 and $\sqrt{n}$. There are other algorithms for primality testing as well which can be found [here](https://en.wikipedia.org/wiki/Primality_test)

   2. Compute $n=pq$

      - $n$ is used as the modulus for both the public and private keys. Its length, usually expressed bits is the length of the key.
      - $n$ is released as a part of the public key.

   3. Compute $\lambda(n)$

      $\lambda$ is the Carmichael???s totient function. It is defined as the smalles positive integer $m$ such that $a^m \equiv 1\ (mod\ n)$ for every integer $a$ between $1$ and $n$ that is co-prime to $n$. In algebraic terms, $\lambda(n)$ is the exponent of the multiplicative group of integers modulo $n$.  If $n$ can be written in a unique way as $n = p_1^{r_1}p_2^{r_2}..$, then $\lambda(n) = lcm(\lambda(p_1^{r_1}),\lambda(p_2^{r_2}),...)$, this can be proven using Chinese remainder theorem. Carmichaels??? theorem states that $\lambda(p^r)$ is equal to euler totient $\phi(p^r)$ if $r$ is power of an odd prime.

      - Since $n=pq$, $\lambda(n) = lcm(\lambda(p),\lambda(q))$ and since $p$ and $q$ are prime $\lambda(p) = \phi(p) = p-1$ and $\lambda(q) = \phi(q) = q-1$. Thus $\lambda(n) = lcm(p-1,q-1)$. lcm can be calculated using the euclidean algorithm.

   4. Choose an integer $e$ such that $1<e<\lambda(n)$ and $gcd(e,\lambda(n)) =1$

      - $e$ having a short bit length and small hamming weight (that is small sum of digits) results in more efficient encryption. The most commonly chosen value for $e$ is $2^16+1=65,537$. The smallest and fastest possible value for e is 3, but it has been shown to be less secure.
      - $e$ is released as a part of public key

   5. Determine $d$ as $d \equiv e^{-1}(mod \ \lambda(n))$

      - Solve for $d$ in the equation $de \equiv 1(mod\ \lambda(n))$, $d$ can be computed efficiently by using the extended euclidean algorithm, since $e$ and $\lambda(n)$ are co prime, the equation is a form B??zout's identity, where $d$ is one of the coefficients.
      - $d$ is kept secret as the private key exponent.

   The public key now consists of the modulus $n$ and the public exponent $e$. The private key consists of the exponent $d$. The values used in generating keys $p,q$ and $\lambda(n)$ must also be kept secret, and are discarded.

2. **Key distribution**
   The receiver sends the public key to encrypt the message to the sender via a reliable but not neccesarily a secure route.

3. **Encryption**
   After the sender receives the receiver???s public key, he now encrypts the message as follows. The sender first converts the message into an integer $m$, such that $0\leq m< n$ by using an agreed upon reversible protocol known as a padding scheme. He then computes the ciphertext $c$ using receiver???s public key $e$ corresponding to $m^e \equiv c\ (mod\ n)$. This is efficient even for very large numbers. This ciphertext is now transmitted to the receiver.

4. **Decryption**
   The receiver can recover $m$ from c by using the private key which consists of the exponent $d$ by computing $c^d \equiv (m^e)^d \equiv m\ (mod\ n)$. Once $m$ is recovered, the original message $M$ is obtained by reversing the padding scheme.

### Padding schemes

**RSA** is prone to multiple attacks such as

- When encrypting with low encryption exponents small values of $m$ then the result of $m^e$ is lessthan the modulus $n$. In this case ciphertexts can be decrypted easily by taking $e$th root.
- An attacker, try out multiple likely plain text messages to check which one is equal to encrypted message after encryption. This is possible since RSA is deterministic and has no random component.

The above are only a few examples of attacks that are possible against RSA. To avoid these problems, RSA typically embed some form of structured randomized padding into value $m$ before encrypting it. There are various standards for padding. An example of which is Optimal Asymmetric Encryption Padding.

## Digital Signature

Another application of **RSA** is digital signature, used for verifying the authenticity of digital messages or documents. Here the sender???s public and private key are used instead of the receiver???s.

1. The sender converts the message M into an integer $m$ using a message digest function like MD5, which is then encrypted with the sender???s public key and sent along with the original message. The encrypted part is known as the **Digital signature**
2. After the receiver receives the original message, sender???s digital signature, It converts the received message into an integer using the same message digest function used above. The digital signature is then decrypted using sender???s public key and compared with the value obtained from the message digest function. If it matches the message is accepted else the message is rejected.
