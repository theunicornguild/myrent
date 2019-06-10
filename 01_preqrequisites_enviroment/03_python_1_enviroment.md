### Creating the environment

We will start by creating our development environment, in the terminal lets create a Development folder:

```bash
mkdir ~/Development
cd ~/Development
```

### Installing virtualenv

Let’s install virtualenv (you can skip this step if you have virutalenv pre-installed):

```bash
pip install virtualenv
```

Then we can create our virtualenv by typing

```bash
virtualenv --python=python3 myrentenv
```

or for windows

```bash
Virtualenv --python=”C:\Python3.7\python.exe” myrentenv
```

### activate the virtualenv

For MacOS/Linux

```bash
source myrentenv/bin/activate
```

For Windows

```bash
myrent\Scripts\activate
```

### Cloning the repo

```bash
git clone https://github.com/test/test.git
```

Now let’s go to the newly cloned project and see what we have there

```bash
cd test
ls -la
```
