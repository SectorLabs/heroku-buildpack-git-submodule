![heroku](http://i.imgur.com/gdl3WtA.png)

This Heroku buildpack adds support for Git submodules. This allows submodules to work with Github Sync. Heroku supports submodules natively, but only through Git pushes. This works around that limitation.

## Why?
Others have attempted to provide this functionality by manually parsing the `.gitmodules` file. This is an unreliable methods. This is because the `.gitmodules` file only describes certain things about the submodules. The exact commit hash a submodule is bound to is actually stored in Git's object tree. Relying solely on `.gitmodules` to fetch submodules is therefor insufficient.

This buildpack takes a different approach and makes using the native Git way of resolving submodules work. It does this by actually fetching the Git objects. Normally this is lost during deployment, but this buildpack works around that. After that, it's a matter of executing `git submodule update --init --recursive`.

## Usage

1. Add the buildpack to your Heroku app:

        heroku buildpacks:add https://github.com/SectorLabs/heroku-buildpack-git-submodule.git

2. Set the `GIT_REPO_URL` to the SSH URL of your Git repo:

        heroku config:set GIT_REPO_URL=git@github.com:SectorLabs/myrepo

3. Set `GIT_SSH_KEY` to the private SSH key that can access both your repo and its submodules:

        heroku config:set GIT_SSH_KEY=$(cat ~/.ssh/id_rsa)

4. Enjoy the ride :)

## Limitations
1. SSH Authentication only.
