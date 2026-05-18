Question:
Why not use RSA alone for all enterprise traffic?

Answer:
RSA and other asymmetric algorithms are computationally expensive and unsuitable for bulk session encryption. Modern TLS uses hybrid cryptography: asymmetric algorithms establish trust and exchange secrets; symmetric algorithms protect ongoing data because they are significantly faster and scalable.

Evidence:
TLSv1.3
ChaCha20-Poly1305
Certificate validation
Local PKI
Handshake inspection
