# GPG - SSH setup

## Generating the master key

Here we create the master key. We want only `Certify` capability: we use the master key only to create the subkeys, `Sign - Encrypt - Authenticate` capabilities will be assigned to the subkeys.

Run the following command to start the master key generation process. Select the `set your own capabilities` creation process (type `8`)

      ▶ gpg --full-generate-key --expert
      gpg (GnuPG) 2.2.9; Copyright (C) 2018 Free Software Foundation, Inc.
      This is free software: you are free to change and redistribute it.
      There is NO WARRANTY, to the extent permitted by law.

      Please select what kind of key you want:
      (1) RSA and RSA (default)
      (2) DSA and Elgamal
      (3) DSA (sign only)
      (4) RSA (sign only)
      (7) DSA (set your own capabilities)
      (8) RSA (set your own capabilities)
      (9) ECC and ECC
      (10) ECC (sign only)
      (11) ECC (set your own capabilities)
      (13) Existing key
      Your selection? 8

Disable sign capability (type `S`)

      Possible actions for a RSA key: Sign Certify Encrypt Authenticate
      Current allowed actions: Sign Certify Encrypt

      (S) Toggle the sign capability
      (E) Toggle the encrypt capability
      (A) Toggle the authenticate capability
      (Q) Finished

      Your selection? S

Disable encrypt capabilities (type `E`)

      Possible actions for a RSA key: Sign Certify Encrypt Authenticate
      Current allowed actions: Certify Encrypt

      (S) Toggle the sign capability
      (E) Toggle the encrypt capability
      (A) Toggle the authenticate capability
      (Q) Finished

      Your selection? E

Quit and continue the creation process (type Q)

      Possible actions for a RSA key: Sign Certify Encrypt Authenticate
      Current allowed actions: Certify

      (S) Toggle the sign capability
      (E) Toggle the encrypt capability
      (A) Toggle the authenticate capability
      (Q) Finished

      Your selection? Q

Input the desired key size of the master key

      RSA keys may be between 1024 and 4096 bits long.
      What keysize do you want? (2048) 4096
      Requested keysize is 4096 bits

Setup the expiration for the master key

      Please specify how long the key should be valid.
            0 = key does not expire
            <n>  = key expires in n days
            <n>w = key expires in n weeks
            <n>m = key expires in n months
            <n>y = key expires in n years
      Key is valid for? (0) 1y
      Key expires at Sat Jul 27 17:59:59 2019 BST
      Is this correct? (y/N) y

Construct your user ID (input your full name and email, leave comment empty). Type `O` to complete

      GnuPG needs to construct a user ID to identify your key.

      Real name: Mattia Cattarinussi
      Email address: example@email.com
      Comment:
      You selected this USER-ID:
      "Mattia Cattarinussi <example@email.com>"

      Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O

Enter a passphrase for your master key

      ┌──────────────────────────────────────────────────────┐
      │ Please enter the passphrase to                       │
      │ protect your new key                                 │
      │                                                      │
      │ Passphrase: ________________________________________ │
      │                                                      │
      │       <OK>                              <Cancel>     │
      └──────────────────────────────────────────────────────┘

If you did the above steps correctly you should have the following result

      We need to generate a lot of random bytes. It is a good idea to perform
      some other action (type on the keyboard, move the mouse, utilize the
      disks) during the prime generation; this gives the random number
      generator a better chance to gain enough entropy.
      gpg: key 4EF9EF4CDBD7AB1B marked as ultimately trusted
      gpg: revocation certificate stored as '/Users/mattiacattarinussi/.gnupg/openpgp-revocs.d/F8DD1C85581AB87675EF97444EF9EF4CDBD7AB1B.rev'
      public and secret key created and signed.

      pub   rsa4096 2018-07-27 [C] [expires: 2019-07-27]
            F8DD1C85581AB87675EF97444EF9EF4CDBD7AB1B
      uid                      Mattia Cattarinussi <example@email.com>

Store the revocation certificate (created by gpg) for your master key on a physical device.

## Generate Sign, Encrypt and Authentication subkeys

Run this command to edit your key

      ▶ gpg --expert --edit-key mattia
      gpg (GnuPG) 2.2.9; Copyright (C) 2018 Free Software Foundation, Inc.
      This is free software: you are free to change and redistribute it.
      There is NO WARRANTY, to the extent permitted by law.

      Secret key is available.

      sec  rsa4096/4EF9EF4CDBD7AB1B
      created: 2018-07-27  expires: 2019-07-27  usage: C
      trust: ultimate      validity: ultimate
      [ultimate] (1). Mattia Cattarinussi <example@email.com>

In the gpg console prompt specify that you want to add a new key for that master key:
      
      gpg> addkey

Select the `set your own capabilities` creation process (type `8`)

      Please select what kind of key you want:
      (3) DSA (sign only)
      (4) RSA (sign only)
      (5) Elgamal (encrypt only)
      (6) RSA (encrypt only)
      (7) DSA (set your own capabilities)
      (8) RSA (set your own capabilities)
      (10) ECC (sign only)
      (11) ECC (set your own capabilities)
      (12) ECC (encrypt only)
      (13) Existing key
      Your selection? 8

Add `Authenticate` capabilities (type `A`). `Sign` and `Encrypt` capabilities are already enabled by default

      Possible actions for a RSA key: Sign Encrypt Authenticate
      Current allowed actions: Sign Encrypt

      (S) Toggle the sign capability
      (E) Toggle the encrypt capability
      (A) Toggle the authenticate capability
      (Q) Finished

      Your selection? A

Type `Q` to continue the process

      Possible actions for a RSA key: Sign Encrypt Authenticate
      Current allowed actions: Sign Encrypt Authenticate

      (S) Toggle the sign capability
      (E) Toggle the encrypt capability
      (A) Toggle the authenticate capability
      (Q) Finished

      Your selection? Q

Input the desired key size for the subkey

      RSA keys may be between 1024 and 4096 bits long.
      What keysize do you want? (2048) 4096
      Requested keysize is 4096 bits

Setup the expiration for the subkey

      Please specify how long the key should be valid.
            0 = key does not expire
            <n>  = key expires in n days
            <n>w = key expires in n weeks
            <n>m = key expires in n months
            <n>y = key expires in n years
      Key is valid for? (0) 1y
      Key expires at Sat Jul 27 18:19:38 2019 BST
      Is this correct? (y/N) y
      Really create? (y/N) y

Input the passphrase for the master key (the one you setup in the master key generation process). You should get this result

      We need to generate a lot of random bytes. It is a good idea to perform
      some other action (type on the keyboard, move the mouse, utilize the
      disks) during the prime generation; this gives the random number
      generator a better chance to gain enough entropy.

      sec  rsa4096/4EF9EF4CDBD7AB1B
      created: 2018-07-27  expires: 2019-07-27  usage: C
      trust: ultimate      validity: ultimate
      ssb  rsa4096/855B567AA2E43D00
      created: 2018-07-27  expires: 2019-07-27  usage: SEA
      [ultimate] (1). Mattia Cattarinussi <example@email.com>

Save and exit

      gpg> save

Now if you list your keys you will see also a subkey (`sub`) with `SEA` capabilities (Sign - Encrypt - Authenticate)

      ▶ gpg --list-keys
      /Users/mattiacattarinussi/.gnupg/pubring.kbx
      --------------------------------------------
      pub   rsa4096 2018-07-27 [C] [expires: 2019-07-27]
            F8DD1C85581AB87675EF97444EF9EF4CDBD7AB1B
      uid           [ultimate] Mattia Cattarinussi <example@email.com>
      sub   rsa4096 2018-07-27 [SEA] [expires: 2019-07-27]

## Setup the gpg-agent for SSH authentication

Enable the `gpg-agent` ssh support

      ▶ echo enable-ssh-support >> $HOME/.gnupg/gpg-agent.conf

Set `SSH_AUTH_SOCK` so that SSH will use `gpg-agent` instead of `ssh-agent`. Add this to tour bashprofile or zshrc
```bash
unset SSH_AGENT_PID
if [ "${gnupg_SSH_AUTH_SOCK_by:-0}" -ne $$ ]; then
  export SSH_AUTH_SOCK="$(gpgconf --list-dirs agent-ssh-socket)"
fi
export GPG_TTY=$(tty)
gpg-connect-agent updatestartuptty /bye >/dev/null
```

Enable the gpg subkey for ssh authentication:
- Get the subkey keygrip

      ▶ gpg --list-keys --with-keygrip

      /Users/mattiacattarinussi/.gnupg/pubring.kbx
      --------------------------------------------
      pub   rsa4096 2018-07-27 [C] [expires: 2019-07-27]
            F8DD1C85581AB87675EF97444EF9EF4CDBD7AB1B
            Keygrip = 22DBE374608C6220B321D3A2543F3806FF63A49D
      uid           [ultimate] Mattia Cattarinussi <example@email.com>
      sub   rsa4096 2018-07-27 [SEA] [expires: 2019-07-27]
            Keygrip = A55719832AF939C531BACFFABB2A47B52FFBBF43

- Add the keygrip of your subkey in the list of approved keys
      
      ▶ echo A55719832AF939C531BACFFABB2A47B52FFBBF43 >> ~/.gnupg/sshcontrol

Check if the key is present in the ssh identities list

      ▶ ssh-add -l
      4096 SHA256:bCVzkgaoGSqJC89hZ/8gclTn7ENN/dJ+mZBBw2zJFuI (none) (RSA)

Retrieve the public ssh key for the subkey

      ▶ gpg --export-ssh-key mattia
      ssh-rsa <A_LOT_OF_STUFF_HERE> openpgp:0xA2E43D00

You can test if the key is working with your Github account. The ssh public key generated in the previous step has to be added to your Github SSH keys.

      ▶ ssh -T git@github.com
      Hi mcattarinussi! You've successfully authenticated, but GitHub does not provide shell access.

# Backup keys and remove the master key

Export the secret master key

      ▶ gpg -a --export-secret-keys > master-secret-key.gpg

Export all the secret subkeys

      ▶ gpg -a --export-secret-subkeys > sub-secret-keys.gpg

Save `master-secret-key.gpg` and `sub-secret-keys.gpg` on a physical device.

Delete the secret keys (you need to delete all the subkeys as well)

      ▶ gpg --delete-secret-key mattia

Enter the master password and confirm the deletion in the subsequent confirmation dialogs.

Restore the subkeys

      ▶ gpg --import sub-secret-keys.gpg

Check the result

      ▶ gpg --list-secret-keys
      /Users/mattiacattarinussi/.gnupg/pubring.kbx
      --------------------------------------------
      sec#  rsa4096 2018-09-14 [C] [expires: 2020-09-13]
            5615F7C581E8450E34F9031703426E5D827D6A81
      uid           [ultimate] Mattia Cattarinussi <mcattarinussi@gmail.com>
      ssb   rsa4096 2018-09-14 [S] [expires: 2018-09-16]
      ssb   rsa4096 2018-09-14 [E] [expires: 2020-09-13]
      ssb   rsa4096 2018-09-14 [A] [expires: 2020-09-13]

The `#` after the master key means that the key is not stored locally.

## References

- https://wiki.archlinux.org/index.php/GnuPG#SSH_agent
- https://www.esev.com/blog/post/2015-01-pgp-ssh-key-on-yubikey-neo/
- https://www.linode.com/docs/security/authentication/gpg-key-for-ssh-authentication/
- https://medium.com/@ahawkins/securing-my-digital-life-gpg-yubikey-ssh-on-macos-5f115cb01266
