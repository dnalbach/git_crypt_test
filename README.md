# Lessons Learned

(Official Git Repo)[https://github.com/AGWA/git-crypt]

After a couple of experiments and repo resets, I've learned that if a file has ever been committed to the repository in an unencrypted state, unfiltered by git-crypt, then it will never be filtered by git-crypt. It is designed to only work on new files being filtered before their initial commit. This makes sense, because otherwise you would have a false sense of security by encrypting files at a point in time that had previously been committed insecurely.

A `.gitattributes` file is used to tell git to filter blob pattern filename matches through git-crypt. This only works if `git-crypt init` has *already been performed*. The order of operations is critical in this sequence, otherwise the files with secrets will be exposed.

## Order

1. install git-crypt on your local system (Ubuntu has a package, Macs can brew install)
1. perform `git-crypt init` in the root of the repo on your local system
1. create a .gitattributes file with lines for `<blob_pattern> filter=git-crypt diff=git-crypt`
1. add the secret files matching the blob pattern
1. make sure the secret files are not tracked by the `.gitignore` file
1. add `crypt.key` to the `.gitignore` file
1. export the encryption key with `git-crypt export crypt.key`
1. review what needs to be committed
1. commit and push up
1. share the `crypt.key` file securely with collaborators
1. they must also install git-crypt and perform `git-crypt unlock crypt.key` from the root of the repo (only once)

## CRITICAL NOTES

- It is critical that you do NOT accidentally encrypt the `.gitattributes` file.
- Files committed before git-crypt init and .gitattributes will NEVER be encrypted.
- There are many additional options for encryption and unlocking, the `crypt.key` pattern in this repo is just one example.

## Daily Use

Once git-crypt is initialized and in use, there is nothing special needed. The encryption and decryption happen automatically and transparently according to the .gitattributes rules. The init, export, and unlock are each only performed once, and never again. Everyone uses git normally.
