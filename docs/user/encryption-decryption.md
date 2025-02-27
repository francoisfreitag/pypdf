# Encryption and Decryption of PDFs

PDF encryption makes use of [`RC4`](https://en.wikipedia.org/wiki/RC4) and 
[`AES`](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algorithms 
with different key length. `pypdf` supports all of them until `PDF-2.0`, which
is the latest PDF standard.

`pypdf` use an extra dependency to do encryption or decryption for `AES` algorithms.
We recommend [`pyca/cryptography`](https://cryptography.io/en/latest/). For Python 3.6
users, you can use [`pycryptodome`](https://pypi.org/project/pycryptodome/) as a fallback.

> Please see the note in the [installation guide](installation.md)
> for installing the extra dependencies if interacting with PDFs that use AES.

## Encrypt

You can encrypt a PDF by using a password:

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("example.pdf")
writer = PdfWriter()

# Add all pages to the writer
for page in reader.pages:
    writer.add_page(page)

# Add a password to the new PDF
writer.encrypt("my-secret-password", algorithm="AES-256")

# Save the new PDF to a file
with open("encrypted-pdf.pdf", "wb") as f:
    writer.write(f)
```
The algorithm can be one of `RC4-40`, `RC4-128`, `AES-128`, `AES-256-R5`, `AES-256`.
We recommend using `AES-256-R5`.

> ⚠️ WARNING ⚠️: pypdf uses `RC4` by default for compatibility if you omit the "algorithm" parameter.
> Since `RC4` is insecure, you should use `AES` algorithms.

## Decrypt

You can decrypt a PDF using the appropriate password:

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("encrypted-pdf.pdf")
writer = PdfWriter()

if reader.is_encrypted:
    reader.decrypt("my-secret-password")

# Add all pages to the writer
for page in reader.pages:
    writer.add_page(page)

# Save the new PDF to a file
with open("decrypted-pdf.pdf", "wb") as f:
    writer.write(f)
```
