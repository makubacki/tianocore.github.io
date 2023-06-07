# [CVE-2023-0466 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-0466)

## Published: 03/28/2023

## Recommendation:

No Impact

According to the CVE description, ‘policy’ processing is disabled by default.
 
For PKCS7, the X509 default parameter is used (NULL is passed in CryptoPkg\Library\BaseCryptLib\Pk\CryptPkcs7VerifyCommon.c)
 
In X509_STORE_CTX_init(), ‘default’ parameter is used (CryptoPkg\Library\OpensslLib\openssl\crypto\x509\x509_vfy.c)
 
For X509 verify, no policy parameter is set (CryptoPkg\Library\BaseCryptLib\Pk\CryptX509.c)
 

# [CVE-2023-0465 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-0465)

## Published: 03/28/2023

## Recommendation:

No Impact

According to the CVE description, ‘policy’ processing is disabled by default.
 
For PKCS7, the X509 default parameter is used (NULL is passed in CryptoPkg\Library\BaseCryptLib\Pk\CryptPkcs7VerifyCommon.c)
 
In X509_STORE_CTX_init(), ‘default’ parameter is used (CryptoPkg\Library\OpensslLib\openssl\crypto\x509\x509_vfy.c)
 
For X509 verify, no policy parameter is set (CryptoPkg\Library\BaseCryptLib\Pk\CryptX509.c)
 

# [CVE-2023-0464 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-0464)

## Published: 03/22/2023

## Recommendation:

No Impact

According to the CVE description, ‘policy’ processing is disabled by default.
 
For PKCS7, the X509 default parameter is used (NULL is passed in CryptoPkg\Library\BaseCryptLib\Pk\CryptPkcs7VerifyCommon.c)
 
In X509_STORE_CTX_init(), ‘default’ parameter is used (CryptoPkg\Library\OpensslLib\openssl\crypto\x509\x509_vfy.c)
 
For X509 verify, no policy parameter is set (CryptoPkg\Library\BaseCryptLib\Pk\CryptX509.c)
 

# [CVE-2023-0286 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-0286)

## Published: 02/08/2023

## Recommendation:

No impact to EDK2 updating will resolve CVE

- OpenSSL 1.1.1t, updated in the edk2-stable202305 stable tag


# [CVE-2023-0215 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2023-0215)

## Published: 02/08/2023

## Recommendation:

- OpenSSL 1.1.1t, updated in the edk2-stable202305 stable tag


# [CVE-2022-4450 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2022-4450)

## Published: 02/08/2023

## Recommendation:

- OpenSSL 1.1.1t, updated in the edk2-stable202305 stable tag


# [CVE-2022-4304 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2022-4304)

## Published: 02/08/2023

## Recommendation:

- OpenSSL 1.1.1t, updated in the edk2-stable202305 stable tag


# [CVE-2022-2097 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2022-2097)

## Published: 07/05/2022

## Recommendation:

EDK2 does not enable AES OCB mode or use the 32-bit AES-NI assembly optimizations.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag


# [CVE-2022-2068 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2022-2068)

## Published: 06/21/2022

## Recommendation:

EDK2 never calls the c_rehash.in script in normal build process.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag


# [CVE-2022-1292 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2022-1292)

## Published: 05/03/2022

## Recommendation:

EDK2 never calls the c_rehash.in script in normal build process.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag


# [CVE-2022-0778 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2022-0778)

## Published: 03/15/2022

## Recommendation:

EDK2 does not use the BN_mod_sqrt() function.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag


# [CVE-2021-4160 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2021-4160)

## Published: 01/28/2022

## Recommendation:

The issue only affects MIPS platforms, and we don’t have MIPS in EDK2.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag


# [CVE-2021-3712 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2021-3172)

## Published: 08/24/2021

## Recommendation:

Analysis has confirmed that the relevant OpenSSL string operations have been used correctly and safely in EDK2.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag


# [CVE-2021-3711 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2021-3711)

## Published: 08/24/2021

## Recommendation:

The SM2 algorithm is not supported by EDK2.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag


# [CVE-2021-3450 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2021-3450)

## Published: 03/25/2021

## Recommendation:

 X509_V_FLAG_X509_STRICT is never set by edk2.

Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag


# [CVE-2021-3449 - OpenSSL](https://nvd.nist.gov/vuln/detail/CVE-2021-3449)

## Published: 03/25/2021

## Recommendation:

 Edk2 TLS supports client mode only. This issue only exists in server mode.

 Until further notice, the following versions of OpenSSL are appropriate to use within the EDK2 CryptoPkg:

- OpenSSL 1.1.1j, updated in the edk2-stable202105 stable tag
- OpenSSL 1.1.1n, updated in the edk2-stable202205 stable tag