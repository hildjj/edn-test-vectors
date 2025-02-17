# CBOR EDN Test Vectors

This repository holds test vectors for the
[CBOR Extended Diagnostic Notation (EDN)](https://www.ietf.org/archive/id/draft-ietf-cbor-edn-literals-16.html)
specification.

## File Format

The .csv files in this project are in the format described by
[RFC 4180](https://www.rfc-editor.org/rfc/rfc4180) (Comma-Separated Values),
with the following exceptions:

- LF (\n) line endings are used instead of the specified CRLF (\r\n) line endings.
- Any line starting with "#" (U+0023 Number Sign) is a comment, and MUST be ignored.
- All CSV files are encoded as UTF8.

In particular, note:

- There will always be a header line, consisting of "op,input,output\n" at the beginning of the file.
- Double quotes are not required, unless the field contains a comma, double
  quote, CR, or LF.
- Double quotes are included in a field by using two double quotes (e.g. """foo""" denotes "foo").

### Fields

The following fields are present in each file:

- **op**: The operation to perform on this line.  Valid operations are:
  - "x": Parse the "input" field as EDN, generating bytes.  Convert the hex
    digits in the "output" field to bytes.  The bytes generated from the input
    field must match the bytes generated from the output field.
  - "=": Parse the "input" field as EDN, generating bytes.  Parse the "output"
    field as EDN, generating bytes.  The bytes generated from the input field
    must match the bytes generated from the output field.
  - "-": Parse the "input" field as EDN, generating bytes.  
    - If there is an "output" field, parse it as EDN.  The bytes generated
      from the input field must NOT match the bytes generated from the output
      field.
    - Else if no "output" field, parsing the input as EDN must fail.
  - Any other operator may be ignored for forward-compatibility.
- **input**: The input for the test.  
  - If the input begins with `h]`, the following text will be hex digits.
    Convert those hex digits to bytes, then decode those bytes as UTF8.
    Finally, parse the resulting string as EDN.  This format is used when the
    input does not fit nicely into the CSV file format (e.g. invalid UTF8
    encoding).
  - Otherwise, parse the string as EDN.
- **output**: The expected output for the test.  Optional for "-" tests.

## Files

- **basic.csv**: basic tests from [edn-abnf](https://github.com/cabo/edn-abnf).
- **encoding-indicators.csv**: tests with encoding indicators from
  [edn-abnf](https://github.com/cabo/edn-abnf).
- **success.csv**: inputs that are expected to be valid, from
  [cbor-edn](https://github.com/hildjj/cbor-edn/)
- **failures.csv**:  inputs that are expected to fail, from
  [cbor-edn](https://github.com/hildjj/cbor-edn/)

These files will likely be combined, augmented, and rearranged through
collaboration.

## License

This repository is intended to be treated as an IETF submission.  Correct
citation is needed here, perhaps to [RFC
5378](https://www.rfc-editor.org/rfc/rfc5378).
