# Post Quantum TLS Research Setup (OpenSSL & Open Quantum Safe)
This repository documments the experimental framework and benchmarking results for evaluating NIST Post-Quantum Cryptography (PQC) Standard digital signatures within the real world systems like TLS 1.3. 

The research specifically focuses on the performance trade-offs between Lattice-based (ML-DSA) and Stateless Hash-based (SLH-DSA) signatures, comparing them against the classical RSA baseline.

## Backround ##
With the rapid evolution of quantum computing, classical cryptographic algorithms such as RSA and ECC are expected to become vulnerable. This project explores NIST-standardized Post-Quantum Digital Signature Algorithms, particularly on
ML-DSA (formerly known as CRYSTALS-Dilithium) and SLH-DSA (formerly known SPHINCS+). The goal is to understand how these algorithms behave in practical environments using modern cryptographic tools.

## Tools & Tech Environment ##
In this very early progress, I have set up the the tools & tech environments such as OpenSSL 3.x, OQS Provider (liboqs), Ubuntu (Virtual Machine), and CMake, GCC

## Current Progress ##
Current milestones achieved are break down below:
1. Successfully built and configured OpenSSL with OQS provider
2. Successfully loaded PQC algorithms such as ML-DSA, Falcon, etc.
3. Sucessfully performed key generation using ML-DSA, signing and verification

*Work in Progress:*
1. TLS 1.3 integration
2. Performance benchmarking

## Next Steps ##
1. Prepare the scripts for key generation, signing, and verification
2. Integrate PQC algorithms into TLS 1.3 and measure performance metrics

Note: detailed step-by-step commands for environment setup can be found in docs/commands.md.
