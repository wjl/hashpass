Hashpass

Generate passwords on-the-fly using a master password and identifiers.

Examples:

	Password for example.com:
		hashpass example.com

	My password for example.com's separate bug tracker:
		hashpass example.com bugs

	Very long admin password for a database:
		hashpass --length 50 database admin

	Password using only letters and numbers for a legacy system:
		hashpass legacy system --type alphanumeric

	Pin number for voice mail which only allows several digits:
		hashpass voicemail --type numeric --length 6

	Print out the master password hash:
		hashpass --print-master-password-hash

	Password for example.com, using the master password hash for 'password':
		hashpass example.com --master-password-hash VNcvd6qzEJUgV7c9ZvpKGLD4JBYopsjo5JNrO2vg3tg6VMTi4N78tKfzMRTy4sqzyc0Yad3qYk4tlSqJowaYHV

This is a pretty simple password manager that works by not storing anything at all, but instead generating passwords on the fly given a secret master password and arbitrary identifiers given on the command line.

In addition, Hashpass goes out of it's way to use a simple, straightfoward algorithm, meaning that it's easy to implement in a different language in just about any language in few pages of code, and it's easy to verify that it's secure.

The algorithm is as follows:

	1. Collect the master password from the user.

	2. Collect the password identifier strings from the user.

	3. Concatenate the master password and the identifier strings, separated by null characters, and normalize it to Unicode NFD.

	4. Take the concatenated plain text and compute the SHA-512 digest over it as UTF-8 data.

	5. Convert to the digest to base-94, base-62, or base-10, as requested by the user.

	6. Give the generated password back to the user, possibly truncating it to the length requested by the user.

Our base-94 digits are the characters '!' to '~' in order. Our base-62 digits are the characters '0' to '9' in order, followed by 'A to 'Z' in order, finally 'a' to 'z' in order. Our base-10 digits are the characters '0' to '9' in order. In any case, our base conversion is done by treating the SHA-512 digest as a little-endian unsigned integer and then repeatedly taking the remainer and dividing.

Master password hashes can optionally be generated and then used to verify the input master password without the necessity of typing the master password twice on every use.

For master password hashes, the algorithm to generate them is as follows:

	1. Collect the master password from the user.

	2. Normalize the master password to Unicode NFD.

	3. Compute the SHA-512 digest over it as UTF-8 data.

	4. Convert the digest to base-62.
