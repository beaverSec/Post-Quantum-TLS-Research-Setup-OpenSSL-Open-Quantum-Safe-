# PQC OpenSSL Setup Commands
This document lists the setup and demo commands used in the Post-Quantum TLS research repository.

## 1. Clone & build liboqs
This is the core library providing quantum-resistant algorithms that we must be built from source.
```bash
git clone https://github.com/open-quantum-safe/liboqs.git
cd liboqs
mkdir build && cd build
cmake ..
make -j$(nproc)
sudo make install
```
You can validate it with the command below, and make sure the library liboqs.so is exists in the system path.
```bash
ls /usr/local/lib | grep oqs
```
<img width="770" height="112" alt="image" src="https://github.com/user-attachments/assets/32892d85-6796-4369-9cfa-2ebb84a415ee" />

## 2. Build load OpenSSL OQC provider
To enable PQC algorithms in OpenSSL 3.x, we utilize the oqsprovider, with the command below
```bash
openssl list -providers -provider-path ./lib -provider oqsprovider
```
Expected output:
```bash
Providers:
  oqsprovider
    name: OpenSSL OQS Provider
    version: 0.12.0-dev
    status: active
```
<img width="952" height="158" alt="image" src="https://github.com/user-attachments/assets/fd5d8d42-e39d-4a73-b064-b2c220efd65a" />

Now, you can see the list of PQC signature algorithms provided by OQS:
```bash
openssl list -signature-algorithms -provider-path ./lib -provider oqsprovider
```
These results below show us which PQC signature algorithms are loaded and ready to use:
```bash
  mldsa44 @ oqsprovider
  p256_mldsa44 @ oqsprovider
  rsa3072_mldsa44 @ oqsprovider
  mldsa65 @ oqsprovider
  p384_mldsa65 @ oqsprovider
  mldsa87 @ oqsprovider
  p521_mldsa87 @ oqsprovider
  falcon512 @ oqsprovider
  p256_falcon512 @ oqsprovider
  rsa3072_falcon512 @ oqsprovider
  falconpadded512 @ oqsprovider
  p256_falconpadded512 @ oqsprovider
  rsa3072_falconpadded512 @ oqsprovider
  falcon1024 @ oqsprovider
  p521_falcon1024 @ oqsprovider
  falconpadded1024 @ oqsprovider
  p521_falconpadded1024 @ oqsprovider
  mayo1 @ oqsprovider
  p256_mayo1 @ oqsprovider
  mayo2 @ oqsprovider
  p256_mayo2 @ oqsprovider
  mayo3 @ oqsprovider
  p384_mayo3 @ oqsprovider
  mayo5 @ oqsprovider
  p521_mayo5 @ oqsprovider
  CROSSrsdp128balanced @ oqsprovider
  OV_Is_pkc @ oqsprovider
  p256_OV_Is_pkc @ oqsprovider
  OV_Ip_pkc @ oqsprovider
  p256_OV_Ip_pkc @ oqsprovider
  OV_Is_pkc_skc @ oqsprovider
  p256_OV_Is_pkc_skc @ oqsprovider
  OV_Ip_pkc_skc @ oqsprovider
  p256_OV_Ip_pkc_skc @ oqsprovider
  snova2454 @ oqsprovider
  p256_snova2454 @ oqsprovider
  snova2454esk @ oqsprovider
  p256_snova2454esk @ oqsprovider
  snova37172 @ oqsprovider
  p256_snova37172 @ oqsprovider
  snova2455 @ oqsprovider
  p384_snova2455 @ oqsprovider
  snova2965 @ oqsprovider
  p521_snova2965 @ oqsprovider
  slhdsasha2128s @ oqsprovider
  slhdsasha2128f @ oqsprovider
  slhdsasha2192s @ oqsprovider
  slhdsasha2192f @ oqsprovider
  slhdsasha2256s @ oqsprovider
  slhdsasha2256f @ oqsprovider
  slhdsashake128s @ oqsprovider
  slhdsashake128f @ oqsprovider
  slhdsashake192s @ oqsprovider
  slhdsashake192f @ oqsprovider
  slhdsashake256s @ oqsprovider
  slhdsashake256f @ oqsprovider
```

## 3. Generate PQC key & sign/verify
Let's check if PQC key generation, signing, and verification work correctly:
```bash
openssl genpkey -algorithm mldsa44 \
  -provider default \
  -provider oqsprovider \
  -provider-path ./lib \
  -out key.pem
```

Sign messages
```bash
echo "hello pqc" > message.txt
openssl pkeyutl -sign \
  -inkey key.pem \
  -in message.txt \
  -out sig.bin \
  -provider default \
  -provider oqsprovider \
  -provider-path ./lib
```

Verify signature
```bash
openssl pkeyutl -verify \
  -inkey key.pem \
  -sigfile sig.bin \
  -in message.txt \
  -provider default \
  -provider oqsprovider \
  -provider-path ./lib
```
Expected output: 
```bash
Signature Verified Successfully
```
<img width="404" height="206" alt="image" src="https://github.com/user-attachments/assets/14a21a3f-e7a4-4ab6-9751-eb5b201e86e1" />



NOTE: This setup was tested on Ubuntu 22.04 VM with OpenSSL 3.0.13 and liboqs 0.12.0-dev.
You can refer back to README for research objectives.
