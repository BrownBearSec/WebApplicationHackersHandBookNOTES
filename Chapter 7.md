# Attacking Session Management

# The Need for state
- HTTP is stateless
- unique session tokens are used to link requests to a user
- The two main vulnerabilities to do with states:
- -> Weakness in the generation of session tokens
- -> Weakness in the handling of session tokens

Alternatives to sessions
- HTTP Authentication - the browser will resubmit the credentials with each request
- Sessionless state mechanisms - The app sends all the necessary information at once in a "binary blob"

# Weakness in Token Generation

- Weak tokens are not just for login, they can apply to: 
- -> Passwords recovery tokens 
- -> Tokens placed in hidden form fields to prevent csrf 
- -> tokens used to give one-time access to protected resources 
- -> persistent tokens used in "remember me" functions 
- -> tokens allowing customers of a shopping application that does not require accounts to retrieve the current status of an existing order

Meaningful tokens
- Some tokens are just parts of the user's info encoded. This can be used to retrieve ids, usernames, roles etc

Predictable Tokens
- Sequential tokens are very bad
- Concealed sequences are bad
- Time dependant tokens are bad
- Weak random number generation are bad

Concealed sequences
- Say a site uses the seemingly random tokens `lwjVJA`, `Ls3Ajg`, `xpKr+A` and `XleXYg`
- -> If these are base64 decoded it is binary gibberish
- --> If this binary is conerted to hex we get `9708D524`, `2ECDC08E`, `C692ABF8`, `5E57962`, this also looks random
- ---> If we subtract each number from the previous one we get `FF97C4EB6A`, `97C4EB6A`, `FF97C4EB6A`, `97C4EB6A`. A repeating pattern
- From this we can tell that the algorithm they use, generates tokens by adding 0x97C4EB6A to the prior value, truncates the result to 32-bit number, Base64 encodes it, and then uses it
- This means we now know all prior and future tokens

Time Dependancy
- if the token is based on the time of creation and not enough entropy has been applied, this can be detected by sending multiple creation requests quickly
- this can lead to easy bruteforcing

Weak Random Number generation
- True random is impossible to generate on a computer
- knowing how the "random generator" works can lead to breaking it.
- Vulns have been found in common frameworks before

Testing the quality of randomness
- You may need to apply hypothesis testing in order to tell if a token is random or not
- this can require at minimum 100 tokens, at max 20,000.

Encrypted Tokens
- This can be decrypted/cracked dependin on the algorithm

ECB Ciphers
- "Electronic Cookbook Cipher" = ECB
- symetric encryption
- plain text may still be visible in the cipher
- The same word will be encryped in the same random text. Repeating random text indicates repeated plaintext

CBC Ciphers
- "Cipher block chaining" = CBC
- The plain text is XORed with an Initialisation vector, which forms a block, this block is used as the IV for the next part of the plaintext, repeat.

# Weaknesses in Session Token handling

Disclosure of Tokens on the Network
- This is a whole section on MitMs which is irrelivent since this is always out of scope

Disclosure of Tokens in logs
- LFI can lead to tokens disclosure

Vulnerable Mapping of Tokens to sessions
- There is no reason why a user would be simultaneousl using to session tokens
- Static tokens, ones that are one per user and dont terminate, are bad.
- tokens may be valid for any user as long as they are valid for one. (bad)

Vulnerable Session Termination
- 