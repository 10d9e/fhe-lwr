# fhe-lwr
thoughts on fully homomophic encryption using learning with rounding (LWR)

In the context of Learning with Rounding (LWR), achieving unbounded Fully Homomorphic Encryption (FHE) would indeed require a mechanism to manage and refresh the "noise" or, more precisely, the rounding errors that accumulate during homomorphic operations. While LWE-based schemes use bootstrapping to reduce noise, the equivalent in LWR-based schemes would need a similar but inherently different method due to the nature of rounding errors.

**Bootstrapping in LWE:**
- In LWE-based schemes, bootstrapping works by homomorphically decrypting and re-encrypting the ciphertext, effectively resetting the noise.
- The decryption circuit itself is evaluated homomorphically, which means that the scheme must be somewhat circular secure because it involves encrypting its own secret keys.

**Managing Rounding Error in LWR:**
- For an LWR-based scheme, an analogous process would involve a way to "refresh" the ciphertexts. Since LWR uses a deterministic rounding error instead of random noise, the refreshing process must account for this.
- The equivalent of bootstrapping would involve constructing a homomorphic decryption circuit that can work with LWR ciphertexts, but this would require careful handling of the rounding. The decryption circuit would need to be able to correct the rounding errors that have accumulated during computations.

**Challenges:**
- One of the key challenges with such a system is that deterministic errors, like those from rounding, behave differently from random errors. When operations are performed on LWR ciphertexts, the errors compound in a predictable way, which might be exploited by an attacker.
- Additionally, unlike LWE, where the error is added, in LWR the error comes from the quantization inherent in the rounding process. This means that a bootstrapping-like operation in LWR would also have to reverse this quantization to some extent, which is non-trivial and does not have a straightforward solution.

*If we were to daydream about a perfect cryptographic world...*

It would be akin to having a self-cleaning oven that could handle all the spills and splatters of baking (computations) without any loss in oven functionality (data integrity) or the need for the baker (user) to intervene. In the cryptographic landscape, this is the Holy Grail — an FHE scheme that could handle unlimited computations without significant performance degradation or security compromises.

In practice, achieving this for LWR-based homomorphic encryption remains theoretical. To date, no fully satisfactory method for unbounded FHE using LWR has been proposed that matches the practicality of bootstrapping in LWE-based schemes. The research is ongoing, and it's possible that advances in lattice-based cryptography or new insights into rounding errors could provide a breakthrough in the future. For now, though, the quest for an efficient and secure unbounded FHE scheme using LWR continues, with the community actively exploring novel techniques and approaches.

In theory, one might think that because Learning with Rounding (LWR) introduces a constant and deterministic error, it would simplify the process of bootstrapping compared to the Learning with Errors (LWE) approach, where the error is random. However, there are several nuances to consider:

1. **Predictability of Error**: The deterministic nature of the error in LWR implies predictability. While this might seem advantageous, it also means that the error does not have the same "smoothing" property as random noise. Random noise can average out over many samples, but deterministic errors can accumulate in a predictable manner, potentially leading to patterns that could be exploited.

2. **Error Growth**: The bootstrapping process in LWE schemes is designed to manage the growth of random noise. In LWR, the rounding error is not growing in the same sense, but it does compound through operations, and this compounding must still be managed. The efficiency gain is not clear-cut because the nature of the error is fundamentally different.

3. **Complexity of Bootstrapping**: Bootstrapping is inherently complex because it involves evaluating the decryption circuit homomorphically. This complexity does not necessarily reduce just because the error is deterministic. You would still need to evaluate a complex function (the decryption circuit) on the ciphertext.

4. **Security Considerations**: The deterministic nature of rounding errors could potentially weaken the security of the scheme if not handled correctly. Since security proofs often rely on the indistinguishability of the noise, a predictable pattern could undermine the security model.

5. **Efficiency Gains**: While an "LWR bootstrap" might be more efficient in terms of error management, the overall efficiency of such a process is not guaranteed. The operation must still ensure that the rounding error does not cause decryption errors, and it must also maintain the security of the underlying cryptographic assumption. These requirements could offset any efficiency gains from the predictability of the error.

*Let's take a moment to think about this...*

The process of bootstrapping in an LWR-based scheme would indeed look different from LWE bootstrapping. If designed properly, it could potentially offer efficiency gains, especially since you could potentially avoid some of the complex Gaussian sampling typically associated with LWE. However, these gains would need to be carefully balanced against the potential security risks and the challenges associated with managing deterministic errors through multiple rounds of computation.

*If we're to stretch our imagination...*

It's like you have a clockwork mechanism (LWR) versus a fluid one (LWE). The clockwork might be more predictable, which seems like it would make things easier to manage — until you realize that clockwork can fall out of sync just as fluid systems can become turbulent. The challenge is not just about managing the error but ensuring that, in doing so, you don't introduce weaknesses or complexities elsewhere in the system.

In summary, while there may be potential for efficiency in an "LWR bootstrap" due to the deterministic nature of errors, realizing this efficiency in practice while maintaining security and correctness is a significant challenge. The idea is intriguing, but it requires further research to determine its viability.

**Bootstrapping on LWR*

So the question then becomes, when bootstrapping on an LWR scheme, would it be possible to use an encrypted value within the context of homomorphic operations to manage or offset errors. Let's consider the feasibility and implications of this approach:

1. **Storing the Error**: In an LWR context, the deterministic error is a function of the secret key and the plaintext, modulated by the rounding process. If you could encrypt this error value homomorphically, you'd essentially have a ciphertext that represents the "offset" needed to correct for the rounding error introduced during encryption.

2. **Clearing the Error**: If you have an encrypted version of the error, you could attempt to subtract this error from the encrypted computation results to correct the result. For this to work, the homomorphic scheme would need to support subtraction operations that are precise enough to correct for the rounding without introducing new errors.

3. **Homomorphic Operations on Error Values**: Since the error values would be encrypted, any operation to correct the error would itself be a homomorphic operation. This could potentially introduce additional noise into the system, albeit in a controlled and predictable manner since the error values are themselves deterministic.

4. **Security**: The security implications of encrypting the error values are non-trivial. If an adversary could learn anything about the error values, it could potentially provide insights into the secret key or the plaintext, depending on the scheme's structure. Therefore, careful cryptographic design would be necessary to ensure that the encrypted error values do not compromise the overall security of the system.

5. **Scheme Complexity**: This approach would increase the complexity of the homomorphic encryption scheme. Each homomorphic operation would now need to account for an additional encrypted value representing the error, which would need to be managed throughout computations.

6. **Feasibility**: For this to be feasible, the homomorphic encryption scheme used must be able to handle the additional complexity and must be designed such that the error "correction" process does not introduce new vulnerabilities or make the scheme impractical due to computational overhead.

*Reflecting on the cryptographic landscape...*

It's like playing a video game where you have a magic potion (the encrypted error) that can heal you (correct the error) every time you take a hit (introduce rounding error). But every time you use the potion, there's a slight chance it might not work perfectly (introducing new errors), and you have to make sure the enemy (adversary) doesn't realize you have this potion, or they'll change their strategy (attack vector).

**What would it look like?**

Formalizing an approach for meta-encrypting error values within a somewhat homomorphic encryption (SHE) scheme to bootstrap an LWR-based FHE system is quite speculative and would push the boundaries of current cryptographic research. However, I can provide a high-level conceptual outline of what such a scheme might entail. This will not be a formal specification, as creating such a scheme would be a novel contribution to the field of cryptography and would require rigorous proof of security and functionality.

Let's denote the following:

- $\( \mathbf{s} \)$ is the secret key.
- $\( \mathbf{a} \)$ is a public vector.
- $\( q \)$ is a large modulus.
- $\( p \)$ is a smaller modulus used for rounding.
- $\( \text{Enc}_{\mathbf{s}}(\cdot) \)$ represents the encryption function under the secret key \( \mathbf{s} \).
- $\( \text{Dec}_{\mathbf{s}}(\cdot) \)$ represents the decryption function under the secret key \( \mathbf{s} \).
- $\( \lfloor \cdot \rceil_p \)$ represents the rounding operation to the nearest multiple of \( p \).

For an LWR-based ciphertext $\( c \)$ corresponding to a message $\( m \)$, we have:

$\[ c = \left\lfloor \frac{p}{q} (\mathbf{a} \cdot \mathbf{s}) \right\rceil + m \mod p \]$

Now, let's introduce an SHE scheme that allows for a limited number of additions and multiplications on ciphertexts. We want to use this SHE to meta-encrypt the error from the LWR scheme.

The idea is to compute the LWR error as:

$\[ e = c - \left( \left\lfloor \frac{p}{q} (\mathbf{a} \cdot \mathbf{s}) \right\rceil \mod p \right) \]$

Then, encrypt this error with the SHE scheme:

$\[ e_{\text{enc}} = \text{Enc}_{\mathbf{s}}(e) \]$

During the computation, if we want to add or multiply ciphertexts and then "refresh" or "bootstrap" the noise, we would homomorphically subtract the encrypted error from the result:

$\[ c_{\text{refreshed}} = \text{HomomorphicOp}(c_{\text{result}}) - e_{\text{enc}} \]$

To perform this operation correctly, the homomorphic subtraction needs to align with the LWR error correction, which is a non-trivial task due to the deterministic nature of the rounding error.

The refreshed ciphertext $\( c_{\text{refreshed}} \)$ would have reduced error and could be further used in computations until the next refresh is needed.

*Let's take a breath and ponder...*

Such a scheme would require the SHE used for error encryption to be compatible with the LWR-based system in terms of the operations it supports and the way it handles errors. It would also require a method for ensuring that the homomorphic operations on the encrypted error do not introduce additional errors that compound beyond control.

The formal expression I've outlined is an oversimplification and would need to be fleshed out with precise mathematical definitions, including how messages are encoded into the system, the exact nature of the rounding, and the construction of the homomorphic operations.

*In an imaginative analogy...*

It's like having a noise-canceling headphone (the SHE scheme) that listens to the background noise (the rounding error) and plays an inverse frequency (encrypted error value) to cancel it out. However, the headphone must be perfectly tuned to the specific noise pattern, and if it misjudges the noise even slightly, it might end up amplifying it instead of canceling it.

In practice, developing such a meta-encryption bootstrapping method would be groundbreaking work in the field of lattice-based cryptography. It would involve both theoretical innovations as well as practical implementations and security analyses to ensure it functions correctly and securely.

**Toy Example**

Alright, let's construct a very simplified toy example that demonstrates the concept of using a somewhat homomorphic encryption (SHE) scheme to manage and correct the deterministic error in a Learning with Rounding (LWR) based encryption system. This example is purely illustrative and lacks the complexity and security features of a real-world system.

**Setup:**
- Let $\( q \)$ be a large modulus, $\( q = 1024 \)$.
- Let $\( p \)$ be a smaller modulus for rounding, $\( p = 32 \)$.
- Let $\( \mathbf{s} \)$ be the secret key, where $\( \mathbf{s} = [s_1, s_2, ..., s_n] \)$ and for simplicity $\( s_i = 1 \)$ for all $\( i \)$.
- Let $\( \mathbf{a} \)$ be the public vector, where $\( \mathbf{a} = [a_1, a_2, ..., a_n] \)$ and for simplicity $\( a_i = 2^i \)$ mod $\( q \)$ for all $\( i \)$.
- The rounding function $\( \lfloor \cdot \rceil_p \)$ rounds to the nearest integer multiple of $\( p \)$.

**LWR Encryption of a Single Bit Message \( m \):**
- Let's say $\( m = 1 \)$.
- Calculate $\( c \)$ without noise for simplicity:
- 
  $\[ c = \left\lfloor \frac{p}{q} (\sum_{i=1}^{n} a_i \cdot s_i) \right\rceil + m \mod p \]$
  
  In this case, since $\( s_i = 1 \)$, the dot product simplifies to the sum of $\( a_i \)$.

**SHE Setup:**
- Let's have an SHE scheme that supports addition and consider it's encryption and decryption functions as $\( \text{Enc}_{\mathbf{s}}(\cdot) \) and \( \text{Dec}_{\mathbf{s}}(\cdot) \)$, respectively.

**LWR Error Calculation and Encryption:**
- The error would be the difference between the unrounded and rounded product:
  
  $\[ e = \left(\frac{p}{q} (\sum_{i=1}^{n} a_i) \mod q \right) - \left\lfloor \frac{p}{q} (\sum_{i=1}^{n} a_i) \right\rceil \mod p \]$
- Encrypt this error with the SHE scheme:
  
  $\[ e_{\text{enc}} = \text{Enc}_{\mathbf{s}}(e) \]$

**Homomorphic Operations:**
- Assume we have two encrypted LWR ciphertexts $\( c_1 \)$ and $\( c_2 \)$ that we want to add homomorphically.
- We add them together:
  
  $\[ c_{\text{result}} = (c_1 + c_2) \mod p \]$
  
- We then subtract the encrypted error from the result homomorphically:
  
  $\[ c_{\text{refreshed}} = c_{\text{result}} - e_{\text{enc}} \]$

**Decryption:**
- The receiver, who possesses the secret key $\( \mathbf{s} \)$, would perform decryption on $\( c_{\text{refreshed}} \)$ to obtain the correct result.

*Let's imagine a basic calculation...*

If $\( n = 2 \)$, $\( a_1 = 2 \)$, and $\( a_2 = 4 \)$, then:

- $\( c = \left\lfloor \frac{32}{1024} (2 + 4) \right\rceil + 1 \mod 32 = \left\lfloor \frac{192}{1024} \right\rceil + 1 = 0 + 1 = 1 \)$ (since $\( \frac{192}{1024} \)$ is less than $\( \frac{1}{2} \)$ of $\( 32 \)$, we round down to 0).
- $\( e = \left(\frac{192}{1024} \mod 1024 \right) - \left\lfloor \frac{192}{1024} \right\rceil = \frac{192}{1024} - 0 = \frac{192}{1024} \)$.
- $\( e_{\text{enc}} = \text{Enc}_{\mathbf{s}}\left(\frac{192}{1024}\right) \)$.

*Upon decrypting...*

The receiver would then decrypt $\( c_{\text{refreshed}} \)$ with $\( \text{Dec}_{\mathbf{s}}(\cdot) \)$ and apply any necessary rounding or scaling to interpret the result as either 0 or 1 (for a bit).

Remember, this toy example omits many practical and security considerations and focuses on the theory of the scheme.

**Simple Python implementation**

Certainly! Let's create a Python toy example that demonstrates the basic idea. This is a highly simplified model and not secure by any means. It's only meant to illustrate the concepts discussed. We'll use basic integer arithmetic to simulate the encryption, homomorphic addition, and decryption processes.

```python
import random

# Parameters
q = 1024
p = 32
n = 4  # Length of the vector a and s

# Secret key s (in a real system, this would be randomly generated)
s = [1] * n  # Secret key

# Public vector a (in a real system, this would be randomly generated)
a = [random.randint(0, q-1) for _ in range(n)]

# Rounding function to the nearest multiple of p
def round_to_p(x, p):
    return round(x / p) * p

# LWR-based encryption function (no noise for simplicity)
def lwr_encrypt(m, a, s, p, q):
    dot_product = sum([a[i] * s[i] for i in range(n)])
    c = round_to_p(dot_product * p / q, p) + m % p
    return c

# SHE-based encryption (just a placeholder for this example)
def she_encrypt(e):
    return e  # In a real scenario, this would be a complex encryption operation

# SHE-based decryption (just a placeholder for this example)
def she_decrypt(e_enc):
    return e_enc  # In a real scenario, this would be a complex decryption operation

# Homomorphic addition of two ciphertexts
def homomorphic_add(c1, c2, p):
    return (c1 + c2) % p

# Example usage
m1 = 1
m2 = 1

# Encrypt two messages
c1 = lwr_encrypt(m1, a, s, p, q)
c2 = lwr_encrypt(m2, a, s, p, q)

# Homomorphically add the encrypted values
c_result = homomorphic_add(c1, c2, p)

# Calculate the error (for illustration, we assume it's the same for both)
e = (c_result - (round_to_p(sum(a), p) + m1 + m2) % p) % p
e_enc = she_encrypt(e)

# "Decrypt" the error using SHE decrypt (placeholder)
e_dec = she_decrypt(e_enc)

# Correct the homomorphic result by subtracting the error
c_corrected = (c_result - e_dec) % p

# Decrypt the corrected ciphertext
m_decrypted = c_corrected % p  # In a real system, this would involve reversing the LWR encryption

print(f"Encrypted m1: {c1}")
print(f"Encrypted m2: {c2}")
print(f"Homomorphically added result: {c_result}")
print(f"Error: {e}")
print(f"Encrypted Error: {e_enc}")
print(f"Corrected ciphertext: {c_corrected}")
print(f"Decrypted message: {m_decrypted}")
```

In this toy example:

- We're using simple integer arithmetic to simulate the encryption and decryption functions. In a real-world scenario, these would be complex functions involving polynomials and coefficient arithmetic.
- The `round_to_p` function is a stand-in for the rounding operation in LWR.
- For simplicity, our SHE encryption and decryption functions (`she_encrypt` and `she_decrypt`) are just placeholders. In a real system, these would involve actual cryptographic operations.
- We homomorphically add two ciphertexts with `homomorphic_add`.
- We calculate and correct the error as if it were encrypted and decrypted homomorphically. In a real system, there would be actual encryption and decryption of the error, which would also introduce and then correct additional noise.

This code will run and demonstrate the basic idea, but remember, this is not a secure implementation. It's purely for educational purposes to illustrate the concept of managing deterministic rounding errors in a hypothetical LWR-based homomorphic encryption scheme.
