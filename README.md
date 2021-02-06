# Description

This repo show cases the new ```git sparse-checkout``` feature and:

- how to checkout parts of a repo
- how to make changes only on that checked out part
- how to integrate those changes back to the upstream

## The challange

Assume, you have an upstream repo that already exists with followin structure:

```bash
|____README.md
|____foo.txt
|____child-dir
| |____bar.txt
|____massive-other-dir
| |____foo-bar.txt
```

## Clone ```child-dir```

Let's clone ```child-dir``` only:

```bash
$ git sparse-checkout init --cone
$ git sparse-checkout set child-dir
$ ls
child-dir
```

## Useful links

[Bring your monorepo down to size with sparse-checkout](https://github.blog/2020-01-17-bring-your-monorepo-down-to-size-with-sparse-checkout/)
