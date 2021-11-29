---
title: Save time integrating git with pass
date: 2020-01-18
description: "Use this recipe to save time if you continuosly are using git online services as Gitlab, Github, Bitbucket. You just need to remember one password and enter it once per session."
image: https://apimania.netlify.app/api/txt2img?text=Save%20time%20integrating%20git%20with%20pass&font=Titillium%20Web&format=4:3
---
Git push has been very frequent lately, almost always associated with some [opensource projects](https://github.com/daxslab) in which I have been participating. And having to enter the username and password every time I go to do a push, it already made me tedious and boring. Some solution has to exist.

Well, yes. And it comes from the hand of [pass](https://www.passwordstore.org/), the standard password manager. This utility allows us to create a database where we can save valuable information, for example, passwords. And everything will be protected by a master password.

## Goal?

Avoid typing the credentials of the code hosting services (github/gitlab/framagit/etc)

## Install pass and configure credentials

pass will be in charge of storing our credentials for each service. First we install, initialize and proceed to save credentials in it.

```bash
sudo apt install pass
pass init <master-password>
pass insert --multiline dev/github
<contraseña>
<nombre de usuario>
Ctrl+D
```

The first of these commands installs pass on our system. Then it is initialized using the master password as a key for everything that we will save here.

Next we proceed to create the first credential record, in this case for Github. And to be organized, we will save it inside the dev folder. That is why the identifier for this credential is `dev / github`. The `--multiline` parameter is specified to be able to save multiple lines in the same record:

* in the first line the password
* in the second line the username that we use to authenticate in that service.

This is a convention used by various applications that use pass. We finish the entry by pressing Ctrl + D.

If we use several code hosting services such as gitlab or another, then we repeat the `pass insert --multiline <identifier>` step.

## Install and configure pass-git-helper

[pass-git-helper](https://github.com/languitar/pass-git-helper) is the utility in charge of establishing the connection between git and pass. To install it we must execute:

```bash
git clone https://github.com/languitar/pass-git-helper.git
cd pass-git-helper
sudo python3 setup.py install
```

Once installed we proceed to its "activation" by executing.

```bash
git config --global credential.helper /full/path/to/pass-git-helper
```

## Map credentials

Once configured, we must specify which credentials should be used in each case. We will achieve this with a mapping file that we are going to create in `~/.config/pass-git-helper/git-pass-mapping.ini` and whose content should be similar to:

```ini
[github.com]
target=dev/github

[gitlab.com]
target=dev/gitlab
```

In brackets we specify the domain of the version control service and then, we assign target the identifier of the credential of the same that we created in step 1.

In other words, when an operation is performed against github.com that requires the credentials in this service, the helper will ask pass for the password stored in dev / github. Remember that the first line of this record is the password and the 2 the user name.

## Conclusions

* it's tedious having to type our credentials every time we do a git clone / pull / push.
* pass is an excellent utility for storing private information.
* It can be managed with QtPass client.
* git-pass-helper allows to integrate git with pass
* the identifier of the credentials is just a name. We assigned `dev/github` to make it clearer
* the mapping file is in charge of defining which credentials (of those stored with pass) will be used to access each of the domains specified between [].

And in this way we save ourselves from having to continually enter our credentials when we are going to interact with this type of service. We have to enter it just once, the first time we open the credentials storage, then no more until we close session or turn of the PC. I'm not sure on this last one. Can you test and comment in the results?
