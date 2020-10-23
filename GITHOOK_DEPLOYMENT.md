# Deploy Documentation to Production with Git Hooks

Git Hooks are used to update the production deployment of the `peach-devdocs` Git book, located at [docs.peachcloud.org](https://docs.peachcloud.org). This document outlines the way in which this semi-automated process is setup and utilised.

The foundational reference used for this process is the Digital Ocean guide: [How To Use Git Hooks To Automate Development and Deployment Tasks](https://www.digitalocean.com/community/tutorials/how-to-use-git-hooks-to-automate-development-and-deployment-tasks).

-----

Assuming the `peach-devdocs` repo has already been cloned to the developer's local machine, the next step is to login to the remote server (where the documentation will be hosted) and create an empty Git repository:

```bash
mkdir ~/devdocs_bare
cd ~/devdocs_bare
git init --bare
```

The `devdocs_bare` directory now contains all the files one would usually find in the `.git` directory of an initialized repository.

Next, create a second directory which will hold the files and assets needed to build the documentation Git book:

```bash
mkdir ~/devdocs_build
```

Ensure the correct permissions are set to allow modifying the web root of the production documentation directory:

```bash
sudo chown -R `whoami`:`id -gn` /var/www/docs.peachcloud.org/html
```

Now the Git hook post-receive script can be created. This script is executed after the `peach-devdocs` repository has been pushed-to from the local instance of the repository. It contains the primary logic of the automation process.

Move into the `devdocs_bare` directory and create the hooks script:

```bash
cd ~/devdocs_bare
vim hooks/post-receive
```

Add the following text to the script:

```bash
#!/bin/bash
while read oldrev newrev ref
do
    if [[ $ref =~ .*/master$ ]];
    then
        echo "Master ref received.  Deploying master branch to build directory..."
        git --work-tree=/home/glyph/devdocs_build --git-dir=/home/glyph/devdocs_bare checkout -f
        echo "Building docs and deploying to production..."
        mdbook build /home/glyph/devdocs_build --dest-dir /var/www/docs.peachcloud.org/html
    else
        echo "Ref $ref successfully received.  Doing nothing: only the master branch may be deployed on this server."
    fi
done
```

_In brief, the script clones the latest documentation code to the `devdocs_build` directory, builds it, and copies the resulting book to the web directory._

The automated build and deployment process can now be initiated from the developer's local machine. First, the production server is added as a remote, then the push is made:

```bash
cd peach-devdocs
git remote add production user@server_ip:devdocs_bare
git push production master
```

Git will request the SSH key in order to push to the server. In the case of `peach-devdocs`, @glyph holds the key. The exact command he uses to push is:

```bash
GIT_SSH_COMMAND='ssh -i ~/.ssh/digiocean/id_rsa -o IdentitiesOnly=yes' git push production master
```

If successful, the command output should look something like the following:

```bash
Counting objects: 269, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (252/252), done.
Writing objects: 100% (269/269), 382.90 KiB | 0 bytes/s, done.
Total 269 (delta 133), reused 0 (delta 0)
remote: Master ref received.  Deploying master branch to build directory...
remote: Building docs and deploying to production...
remote: 2020-10-22 10:23:34 [INFO] (mdbook::book): Book building has started
remote: 2020-10-22 10:23:34 [INFO] (mdbook::book): Running the html backend
To 46.101.172.118:devdocs_bare
 * [new branch]      master -> master
```

As can be seen, the `echo` commands from the `hooks/post-receive` script appear in the local terminal output.

The `docs.peachcloud.org` documentation Git book is now up-to-date.
