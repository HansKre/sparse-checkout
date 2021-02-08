# Description

This repo showcases the new ```git sparse-checkout``` feature and:

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

## How to clone ```child-dir``` only (the _OLD_ way)

Let's clone ```child-dir``` only.
The approach is a little bit different from the regular ```git clone <remote-url>``` approach. We need to init a fresh repo locally:

```bash
mkdir clone
cd clone
git init
```

In that empty repo, we define the remote and fetch all objects but do not check them out. So the folder remains empty.

```bash
git remote add -f github git@github.com:HansKre/sparse-checkout.git
```

Check the history to make sure it is fetched:

```bash
git log --oneline
```

Now, enable the sparse-checkout:

```bash
git config core.sparseCheckout true
```

Next, we specify the folders that we want to be part of the sparse-checkout by adding them to ```.git/info/sparse-checkout```

```bash
echo "child-dir" >> .git/info/sparse-checkout
```

We can finally update our local copy by pulling from upstream:

```bash
git pull github master
```

And yep, it worked:

```bash
$ tree
|____child-dir
| |____bar.txt
```

## Clone ```child-dir``` with git version ```2.25.0```

Git ```2.25.0``` includes a new experimental ```git sparse-checkout``` command. Here's how to use it:

```bash
$ git sparse-checkout init --cone
$ git sparse-checkout set child-dir
$ ls
child-dir
```

That's much easier!

## Make changes

```bash
$ echo "some changes" > child-dir/bar.txt
$ git status
modified:   child-dir/bar.txt
git add .
git commit -m "Make changes on sparse-checkout"
git push -u github master
```

If we go to [child-dir/bar.txt](https://github.com/HansKre/sparse-checkout/blob/master/child-dir/bar.txt), we can see that the changes have been successfully added to the repo.

## Filtering aka ```partial clones```

On larger repos you might find filtering aka ```partial clones``` useful:

```bash
git clone --filter=blob:none --no-checkout git@github.com:HansKre/sparse-checkout.git
```

Now, you can continue with the ```sparse-checkout``` config to pull only desired folders. Or, of course, you could checkout the entire repo but with the filter applied.

## Useful links

[Bring your monorepo down to size with sparse-checkout](https://github.blog/2020-01-17-bring-your-monorepo-down-to-size-with-sparse-checkout/)
