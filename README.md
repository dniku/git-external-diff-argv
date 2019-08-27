This repository demostrates what might be a bug for `git-diff`, originally investigated in [this report](https://github.com/jupyter/nbdime/issues/497).

Using an external tool to compare files across branches:

```bash
$ env GIT_EXTERNAL_DIFF=./print_argv.py git diff origin/branch1:file1.txt origin/branch2:file2.txt
['./print_argv.py',
 'file1.txt',
 '/tmp/QRaIJ1_file1.txt',
 '802b1c4ed7b06162b2ce09b7db72a576695b96e5',
 '100644',
 '/tmp/AZuOJ1_file2.txt',
 '076e8e37a712d8a66c0c3d1a103050dc509ca6ff',
 '100644',
 'file2.txt',
 'index 802b1c4..076e8e3 100644\n']
```

`git` passes an unexpected set of parameters to `GIT_EXTERNAL_DIFF`. According to [docs](https://www.git-scm.com/docs/git/2.22.0), it should pass 7 parameters:

```
path old-file old-hex old-mode new-file new-hex new-mode
```

Tested with git 2.22.0 and 2.17.1.
