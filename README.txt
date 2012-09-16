Hashpass

Generate passwords on-the-fly using a master password and identifiers.

Examples:
  Password for example.com:
    hashpass example.com

  My password for example.com's separate bug tracker:
    hashpass example.com bugs

  Very long admin password for a database:
    hashpass --length 50 database admin

  Pin number for voice mail which only allows several digits:
    hashpass voicemail --type numeric --length 6

This is a pretty simple password manager that works by not storing anything at
all, but instead generating passwords on the fly given a secret master password
and arbitrary identifiers given on the command line.

In addition, Hashpass goes out of it's way to use a simple, straightfoward
algorithm, meaning that it's easy to implement in a different language in just
about any language in few pages of code, and it's easy to verify that it's
secure.

The algorithm is as follows:

	1. Collect the master password from the user.

	2. Collect the password identifier strings from the user.

	3. Concatenate the master password and the identifier strings, separated by
		 null characters.

	4. Take the concatenated plain text and compute the SHA-512 digest.

	5. Convert to the digest to base-94 or base-10, as requested by the user.

	6. Give the generated password back to the user, possibly truncating it to
		 the length requested by the user.

Our base-94 digits are the characters '!' to '~' in order; our base-10 digits
are the characters 0 to 9 in order. Either way, our base conversion is done by
treating the SHA-512 digest as a little-endian unsigned integer and then
repeatedly taking the remainer and dividing.
