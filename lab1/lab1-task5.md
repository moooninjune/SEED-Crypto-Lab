## Task 5: Error Propagation – Corrupted Cipher Text

1.	Create a text file that is at least 1000 bytes long.

2.	Encrypt the file using the AES-128 cipher.
```bash
openssl enc -aes-128-ecb -e -in longfile.txt -out ecb_enc -K 00112233445566778899aabbccddeeff
# only ECB doesn't need -iv
# repeat the process for different modes
```

3.	**A single bit of the 55th byte in the encrypted file got corrupted.** You can achieve this corruption using the hex editor `bless filename`.

4.	Decrypt the corrupted ciphertext file using the correct key and IV (SAME KEY).
```bash
openssl enc -aes-128-ecb -d -in ecb_enc -out ecb_dec -K 00112233445566778899aabbccddeeff
```

- How much information can you recover by decrypting the corrupted file, if the encryption mode is `ECB`, `CBC`, `CFB`, or `OFB`?

| Mode | Effect | Explanation |
| ---- | ------ | ----------- |
| ECB | ![ECB]( | **No error propagation**, only the block containing the corrupted byte is affected. The rest of the text remains intact. |
| CBC | ![CBC](https://github.com/moooninjune/SEED-Crypto-Lab/blob/27280ba58e7a85201a06feac9f59b915b46799be/images/lab1-task5-cbc.png) | The corrupted block and the following block become unreadable. This mode shows a more significant impact because of the chaining nature. |
| CFB | ![CFB]( | Like CBC, the corrupted byte affects both the corrupted block and the next blocks, leading to ***more*** distortion. |
| OFB | ![OFB]( | **No error propagation**, only the block containing the corruption is affected, while subsequent blocks remain unaffected. |