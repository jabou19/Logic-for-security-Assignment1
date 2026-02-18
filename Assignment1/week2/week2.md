### an informal outline  which design the protocol in natural language 

- A: resource owner (photos stored at B). No public/private key pair. A authenticates via IP using a weak password (not a good cryptographic secret → so protocol must not use it as an encryption key).
- B: resource server (photo storage). Has a public/private key pair.
- P: photo book service. Has a public/private key pair.
- IP: identity provider (trusted, honest). Has a public/private key pair; everyone knows IP's public key. 
- Pk: public key; IP knows everyone’s public keys. Signed statements by IP are accepted as true. 
- Goal: P gets read-only access to exactly A’s photos at B. no access to other A-resources (email etc.).

### the flow of the protocol:

-  A → P (request): A asks P to access photos at B (with scope: read-only, folder path,).
- P → IdP (authorization request): P asks IdP for an authorization to access A’s photos at B, including requested scope and P’s identity (signed by P).
- IdP ↔ A (consent + login): IdP authenticates A via password and gets consent for requested scope and P. No crypto with the password; password only for authentication at IdP.
- IdP → P (token): IdP issues a signed authorization token (capability) for P. Token includes: A, P, B, scope (read-only, folder), (or similar anti-replay), and is bound to P as the intended audience.
- P → B (access): P presents token to B, signed by IdP. B verifies IdP signature and scope; if valid, grants read-only access to the specific folder.
