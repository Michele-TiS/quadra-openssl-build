# Quadra OpenSSL Build Farm 🛡️

Automated, FIPS 140-3 compliant build pipeline for OpenSSL native binaries, designed specifically for the Quadra cryptographic framework.

## 🎯 Purpose
This repository serves as a strictly verifiable supply chain for OpenSSL binaries. By leveraging isolated, ephemeral GitHub Actions runners, it guarantees that all cryptographic modules are built in a sterile environment directly from the official source code, adhering to FIPS compliance requirements and eliminating the "works on my machine" vulnerability.

## 🔒 FIPS Compliance & Chain of Custody
To maintain an unbreakable Chain of Custody, this build pipeline implements the following mandatory security measures:

1. **Official Source Procurement:** Source tarballs are downloaded directly from the official OpenSSL release endpoints.
2. **Strict Cryptographic Verification:** Before any extraction or compilation occurs, the tarball's SHA-256 hash is strictly validated against the officially published checksum. If the hash diverges by a single bit, the build pipeline is immediately aborted.
3. **Ephemeral Environments:** All binaries are cross-compiled on fresh, completely isolated virtual machines (Ubuntu, Windows Server, macOS) hosted by GitHub, ensuring no environmental contamination.
4. **FIPS Configuration Enforced:** The configuration explicitly enforces the `enable-fips` flag across all platforms to generate the `fips` provider (`.dll`, `.so`, `.dylib`) and compute its internal HMAC checksum (`fipsmodule.cnf`).

## 📦 Supported Architectures
The pipeline concurrently generates static/shared libraries and the FIPS module for the following platforms:
- **Windows:** x64 (MSVC)
- **Linux:** x64, ARM64 (GCC / Cross-compiled)
- **macOS:** ARM64 / Apple Silicon (Clang)
- **Android:** ARM64 (NDK API 29)
- **iOS:** ARM64 (Xcode SDK)

## 🔍 Artifact Verification
Every automated release includes a `sha256sums.txt` ledger. When downloading a release zip file to integrate into the Quadra workspace, you must verify the integrity of the archive to ensure secure transport:

```bash
# Verify integrity on Linux/macOS
sha256sum --check sha256sums.txt
```

```powershell
# Verify integrity on Windows (PowerShell)
Get-FileHash -Algorithm SHA256 .\windows_x64.zip
```
