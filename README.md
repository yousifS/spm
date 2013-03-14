Simple Password Manager (spm)
=============================

--------
Comments
--------
The problem I wanted to solve:
- Passwords stored on one file on a usb that I carry arround.
- Easy way to modify and get stored passwords
- It should work for any recent linux distribution
- It should work for any recent mac os (I have not tested this yet)
- It should work with out running X 
- Learn python

I have tried these before:
- Encrypte a file that had all my passwords using gpg.
- Gnome key manager
- Pass written by Jason A. Donenfeld 

I used slowAES because:
- I wanted a single file for the code base
- Reduce external code dependancies

I used a flat file db because:
- This was the simplest to implement. 
- It fits my needs for now.


-----------------
Structure/Design
-----------------
- Only the master password is stored in memory.
- Digest of the master password is stored in the db (not really necessary)
- The database file is updated after each call that updates the password record:
 - add password
 - delete password
 - update passwrd, username, ref, and notes
 - update master password
- The username, notes, and ref are not encrypted. Only the password is encrypted.
- The password is encrypted using the the master password (padded to 16, 24, or 32 bytes)
- if the master password is greater than 32 bytes only the first 32 bytes are used for encryption.
- we could use md5 digest of the master password as the AES password instead of 5 and 6 above.
- script checks the actual (unpadded full) master password before it runs.

--------------
DB FILE FORMAT 
--------------

1: last used password id, this is incremented on adding a new password

2: master password digest

3: salt used to get master password digest 

4: number of iterations used to get password digest


5: password record id 

6: username 

7: encrypted password (using master password)

8: refence for the password

9: notes for the password


10: password record id 

11: username 

12: encrypted password (using master password)

13: refence for the password

14: notes for the password

.
.
.
