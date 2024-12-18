# MrLou Modules

## Overview
The `mrlou_modules` is a collection of Python package that I keep re-using and thought would be good to share them

## Contributing
If you would like to contribute to this project, please fork the repository and submit a pull request with your changes. Ensure that your code follows the existing style and includes appropriate tests.

## License
This project is licensed under the MIT License. See the LICENSE file for details.

## Installation

You can install the package from PyPI using pip:

```
pip install mrlou_modules
```

# Random_Message

## Overview

The `Random_Message` in `mrlou_modules` is a Python package that provides a collection of random messages, including fun facts and jokes. 
This package is designed to be easy to use and integrate into your Python projects to add a touch of randomness and fun.
I'm using it in script that takes a bit long to complete so printing out those messages keep me awake

## Usage
To use the package in your Python script, follow these steps:

1. Import the necessary function:

```
from mrlou_modules.Random_Message.random_message import get_random_message
```

2. Get a random message:

```
message = get_random_message()
print(message)
```

The `get_random_message` function will alternately return a joke or a fun fact each time it is called.

## Example
Here’s a simple example of how to use `mrlou_modules`:

```
from mrlou_modules.Random_Message.random_message import get_random_message

# Print a random message
print(get_random_message())
```

### Functions
- `get_random_message()`: Returns a random message, alternating between jokes and fun facts.

### Modules
- `fun_facts.py`: Contains a list of interesting and random fun facts. Example facts include:

- `jokes.py`: Contains a list of light-hearted programming jokes. Example jokes include:

- `random_message.py`: Provides the get_random_message function that alternates between fun facts and jokes.

# Cyberark

The `cyberark_api` is a collection of one API for now :-)
The `variables.py` contain all the configuration values such as URIs, certificate paths, and usernames
the `cyberark_api.py` has one class with a function `get_credentials` that you can use to pull credential stored in Cyberark

## Example
Here’s a simple example of how to use `mrlou_modules`:

create a variables.py with your variables by adjusting the <>:

for example, 
AppID = "<AppID>" to AppID = "MyAppIDexample"
ca_cert = r'C:\path\to\Trusted_Root_Certificates.pem' to ca_cert = r'C:\cert\cyberark\Trusted_Root_Certificates.pem'

```
# variables.py

# CyberArk API configurations

URI = "https://<IIS_Server_Ip>/AIMWebService/api/Accounts"
AppID = "<AppID>"
Safe = "<Safe>"
Folder = "<Folder>"
Username = "<Username>"
Object = f"<Object>{Username}"


ca_cert = r'C:\path\to\Trusted_Root_Certificates.pem'
client_cert = r'C:\path\to\_unencrypted_device.crt'
client_key = r'C:\path\to\_unencrypted_key.pem'
```

The main script is 
```
import variables
from mrlou_modules.Cyberark.cyberark_api import CyberArkAPI

cyberark_api = CyberArkAPI(variables)
credentials = cyberark_api.get_credentials()

if credentials:
    print(f"Username: {credentials['Username']}")
    print(f"Password: {credentials['Password']}")
    print(f"Password Change In Process: {credentials['PasswordChangeInProcess']}")

```

# Certificate_Utils

This guide explains how to use the cert_utils.py module, which provides tools for working with certificates and private/public keys. It includes functions to extract certificate details, decrypt private keys, verify certificates, and more.

## Functions Overview

1. extract_common_name(subject)
   - Extracts the Common Name (CN) from a certificate subject.
   - Input: A certificate subject (x509.Name object).
   - Output: The Common Name (CN) as a string, or None if not found.


2. extract_san_extension(certificate)
   - Extracts the Subject Alternative Names (SAN) from a certificate.
   - Input: A certificate (x509.Certificate object).
   - Output: A list of SAN names as strings or None if not available.


3. decrypt_and_save_private_key(file_path, passphrase, output_path)
   - Decrypts an encrypted private key and saves it as an unencrypted PEM file.
   - Input:
     - file_path: Path to the encrypted private key file.
     - passphrase: The passphrase to decrypt the key.
     - output_path: Path to save the unencrypted private key.
   - Output: Saves the unencrypted private key to the specified output path. Prints success or failure messages.


4. process_certificate(input_data, passphrase=None)
   - Reads and processes a certificate or key from a file path or a PEM-encoded string.
   - Input:
     - input_data: A file path or a PEM-encoded string.
     - passphrase: Optional passphrase to decrypt private keys.
   - Output: Depending on the content (certificate, public/private key), returns key or certificate details (e.g., validity, public key) or prints an error if invalid.


5. find_cert_and_key_files(base_path)
   - Searches the given directory (base_path) for certificate and key files.
   - Input: The base directory path to search.
   - Output: A tuple with paths to the certificate and key files.


6. process_certificate_and_key(cert_file_path, key_file_path, passphrase=None)
   - Verifies whether the private key matches the certificate.
   - Input:
     - cert_file_path: Path to the certificate file.
     - key_file_path: Path to the private key file.
     - passphrase: Optional passphrase to decrypt the private key.
   - Output: Prints whether the private key matches the certificate.


7. get_public_key_from_private_key(private_key)
   - Extracts the public key from a private key.
   - Input: A private key (cryptography key object).
   - Output: The public key associated with the private key.


8. compare_public_keys(cert_public_key, key_private_key)
   - Compares the public key from a certificate with the public key derived from a private key.
   - Input:
     - cert_public_key: Public key from the certificate.
     - key_private_key: Private key to derive the public key from.
   - Output: Returns True if the public keys match, otherwise False.

## Example Usage

Extract Common Name and SAN from a Certificate

```python
from cryptography import x509
from mrlou_modules.Certificate_Utils.cert_utils import extract_common_name, extract_san_extension

# Load certificate
with open('certificate.pem', 'rb') as cert_file:
    cert_data = cert_file.read()
    cert = x509.load_pem_x509_certificate(cert_data)

# Extract CN and SAN
common_name = extract_common_name(cert.subject)
san_names = extract_san_extension(cert)

print(f"Common Name: {common_name}")
print(f"Subject Alternative Names: {san_names}")
```

Decrypt and Save a Private Key

```python
from mrlou_modules.Certificate_Utils.cert_utils import decrypt_and_save_private_key

decrypt_and_save_private_key('encrypted_key.pem', b'passphrase', 'decrypted_key.pem')
```

Process Certificate or Key

```python
from mrlou_modules.Certificate_Utils.cert_utils import process_certificate

cert_details = process_certificate('certificate.pem')
print(cert_details)

# Or process a PEM-encoded string
pem_string = """-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----"""
cert_details = process_certificate(pem_string)
print(cert_details)
```

Verify Certificate and Private Key Match

```python
from mrlou_modules.Certificate_Utils.cert_utils import process_certificate_and_key

process_certificate_and_key('certificate.pem', 'private_key.pem')
```

Find Certificate and Key Files

```python
from mrlou_modules.Certificate_Utils.cert_utils import find_cert_and_key_files

cert_path, key_path = find_cert_and_key_files('/path/to/certs')
print(f"Cert path: {cert_path}, Key path: {key_path}")
```

Load the PKCS#12 file and export the root/intermediate certificates and private key to PEM

```python
from mrlou_modules.Certificate_Utils.cert_utils import convert_pkcs12_to_x509_with_root_chain

p12_path = r"path/to/your_certificate.p12"
p12_password = input("type your password: ")
cert_out_path = r"path/to/output_certificate.pem"
key_out_path = r"path/to/output_private_key.pem"
rootchain_out_path = r"path/to/output_root_chain.pem"

convert_pkcs12_to_x509_with_root_chain(
    p12_path=p12_path,
    p12_password=p12_password,
    cert_out_path=cert_out_path,
    key_out_path=key_out_path,
    rootchain_out_path=rootchain_out_path,
)
```


Load the PKCS#12 file and export all certificates and private key to PEM

```python
from mrlou_modules.Certificate_Utils.cert_utils import convert_pkcs12_to_x509_with_root_chain

p12_path = r"path/to/your_certificate.p12"
p12_password = input("type your password: ")
cert_out_path = r"path/to/output_certificate.pem"
key_out_path = r"path/to/output_private_key.pem"
rootchain_out_path = r"path/to/output_root_chain.pem"

convert_pkcs12_to_x509_with_root_chain(
    p12_path=p12_path,
    p12_password=p12_password,
    cert_out_path=cert_out_path,
    key_out_path=key_out_path,
    rootchain_out_path=rootchain_out_path,
)
```

Extract the Common Name from a pfx certificate

```python
from mrlou_modules.Certificate_Utils.cert_utils import extract_cn_from_pfx

p12_path = r"path/to/your_certificate.p12"
p12_password = input("type your password: ")

extract_cn_from_pfx(
    p12_path,
    p12_password
)
```

Convert cer to pem certificate

```python
from mrlou_modules.Certificate_Utils.cert_utils import convert_cer_to_pem

cer_path = r"path/to/your_certificate.cer"
pem_path = r"path/to/certificates"
convert_cer_to_pem(cer_path, pem_path)
```

Convert crt to pem certificate

```python
from mrlou_modules.Certificate_Utils.cert_utils import convert_crt_to_pem

crt_path = r"path/to/your_certificate.cer"
pem_path = r"path/to/certificates"
convert_crt_to_pem(crt_path, pem_path)
```

Extract root cert from pem concatenate file to individual pem files

```python
import mrlou_modules.Certificate_Utils.cert_utils as cu

# Example usage:
input_pem_file = r'path/to/your_certificate.pem'
output_dir = r'path/to/certificates'

# Call the function to extract and save certificates
cu.extract_and_save_certificates(input_pem_file, output_dir)
```

# Ping Sweep
This script will ping each ip of a given subnet or multiple subnets

import it and run it

```python
from mrlou_modules.Ping_Sweep import ping_sweep

ping_sweep
```
