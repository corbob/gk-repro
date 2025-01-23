This contains reproduction steps and environment for the GPG signing issue I'm seeing.

You don't need Vagrant to reproduce this, but it certainly helps.

1. run `vagrant up` to stand up the VM and install the software in it.
1. Once that's done, launch a PowerShell window in the VM console.
1. Run `ipmo c:\programdata\chocolatey\helpers\chocolateyprofile.psm1;refreshenv` to ensure the path is correct.
1. Run `gpg --ful-generate-key` and press enter through the prompts to accept the defaults until "Is this correct?" which you should answer `Y` to.
1. Provide a "Real name" of `test user`, and an email of `test@test.test`.
1. When prompted create a password for the GPG key.
1. It will output something like the below. Copy the hex key that is presented (in this sample it is `76BA9C254D36089A7E6F34786FE8C20A17B40A1F`)
  ```
  gpg: C:\\Users\\vagrant\\AppData\\Roaming\\gnupg\\trustdb.gpg: trustdb created
  gpg: directory 'C:\\Users\\vagrant\\AppData\\Roaming\\gnupg\\openpgp-revocs.d' created
  gpg: revocation certificate stored as 'C:\\Users\\vagrant\\AppData\\Roaming\\gnupg\\openpgp-revocs.d\\76BA9C254D36089A7E6F34786FE8C20A17B40A1F.rev'
  public and secret key created and signed.
  
  pub   ed25519 2025-01-23 [SC]
        76BA9C254D36089A7E6F34786FE8C20A17B40A1F
  uid                      test user <test@test.test>
  sub   cv25519 2025-01-23 [E]
  ```
1. Run `git config --global user.signingkey 76BA9C254D36089A7E6F34786FE8C20A17B40A1F`
1. Run `git config --global commit.gpgsign true`
1. Open GitKraken and do the intro repository up to attempting to commit changes.

If you do not have vagrant, you can run the following commands in an administrative PowerShell in lieu of Step 1 above:

```powershell
irm ch0.co/go | iex
choco install git.install -y --no-progress
choco install gitkraken -y --no-progress --ignore-checksum
choco install gpg4win -y --no-progress
ipmo /programdata/chocolatey/helpers/chocolateyprofile.psm1
refreshenv
git config --global user.name 'test user'
git config --global user.email 'test@test.test'
```
