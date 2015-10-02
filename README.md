# SSH with ed25519 (*curve25519*) + GNOME-KeyRing

## Overview

At the time of writing *GNOME-Keyring* is - after months of knowledge about this "*bug*" - still unable to use *ed25519* SSH keys. -> https://bugzilla.gnome.org/show_bug.cgi?id=641082
Therefore I had searched for a few hours to find a "*solution*" for this problem.

## Starting Point

- GNOME-Keyring
- OpenSSH v6.5 (or later)

\*) *It might be that the behaviors with the upcoming passphrase-request dialogues may differ depending on your desktop environment and already installed passphrase dialogue handlers. Also it depends on you if you decide (for yourself) to do the right thing and use a (32+ length) passphrase for your SSH key.*

## Generating an *ed25519* SSH key

In the end it's as simple as always.
To generate an *ed25519* SSH key simply open your favorite shell and do this and the following dialogues:

` ssh-keygen -t ed25519 -C "ACommentIfYouWishToHaveOne" `

Info: You don't need to specify any key size because it is already fixed to 256 bits.

## How to enable *ed25519* SSH keys in GNOME-Keyring?

Simply said: You don't. Just leave it as it is.

Do this instead:

1. Install the keychain package.
- If you use a (good) shell like "fish shell" you have to remove the $ in the following command:
  * Do ` eval $(keychain --eval -Q --quiet id_ed25519) `
    - You can add as many keys as you want after *id_ed25519*. Just separate them with *whitespaces*!
    - Notice: It might happen that (several) windows will pop up to request the passphrase for the added key(s). Remove the wrong one that might pop up via pacman.\*
- Afterwards do ` keychain --agents ssh,pgp ` to set Keychain as new default storage for ssh and pgp keys.

Done.

\*) *It might be that the behaviors with the upcoming passphrase-request dialogues may differ depending on your desktop environment and already installed passphrase dialogue handlers. Also it depends on you if you decide (for yourself) to do the right thing and use a (32+ length) passphrase for your SSH key.*

## Testing

If everything worked for you so far you eventually want to test if it cycles.

To do so you could try `keychain -l` first to see your stored keys.

If you can see your recently added key(s) everything should be fine.

To finallize your testing, why not do the [Github tutorial test after the generation of a SSH key](https://help.github.com/articles/generating-ssh-keys/#step-5-test-the-connection)?
Just type `ssh -T git@github.com` if you have also already imported your new *ed25519* SSH key to Github.
You might be asked for a passphrase if you (*hopefully*) stated one.

## Detailed Reading

- [Arch Linux Wiki - SSH keys - Generating an SSH key pair](https://wiki.archlinux.org/index.php/SSH_keys#Generating_an_SSH_key_pair)
- [Arch Linux Wiki - SSH keys - Keychain](https://wiki.archlinux.org/index.php/SSH_keys#Keychain)

## Bugs / ToDos

[] *Found out later* - It seems that keychain forgets about it's stored identities after a period of time..? Due to the storage of the SSH key in the default folder (`~\.ssh`) it probably only needs the passphrase again to use it without problems.
