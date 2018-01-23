![license](https://camo.githubusercontent.com/7564c4f51086d05c91ab09865b0ab1eaf457f172/68747470733a2f2f696d672e736869656c64732e696f2f3a6c6963656e73652d6d69742d626c75652e737667) ![heroku stack](https://img.shields.io/badge/heroku%20stack-cedar14-green.svg)

![heroku](http://i.imgur.com/gdl3WtA.png)

This Heroku buildpack adds support for Git submodules. This allows submodules to work with Github Sync. Heroku supports submodules natively, but only through Git pushes. This works around that limitation.

## Why?
Others have attempted to provide this functionality by manually parsing the `.gitmodules` file. This is an unreliable methods. This is because the `.gitmodules` file only describes certain things about the submodules. The exact commit hash a submodule is bound to is actually stored in Git's object tree. Relying solely on `.gitmodules` to fetch submodules is therefor insufficient.

This buildpack takes a different approach and makes using the native Git way of resolving submodules work. It does this by actually fetching the Git objects. Normally this is lost during deployment, but this buildpack works around that. After that, it's a matter of executing `git submodule update --init --recursive`.

## Usage

1. Add the buildpack to your Heroku app:

        heroku buildpacks:add https://github.com/SectorLabs/heroku-buildpack-git-submodule.git
        
    Keep in mind that the buildpack order is important. If you'll specify this buildpack after your default one (e.g. `heroku/nodejs`) it'll not work. See [https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app](https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app) for details.

2. Set the `GIT_REPO_URL` to the SSH URL of your Git repo:

        heroku config:set GIT_REPO_URL=git@github.com:SectorLabs/myrepo

3. Set `GIT_SSH_KEY` to the private SSH key that can access both your repo and its submodules:

        heroku config:set GIT_SSH_KEY=$(cat ~/.ssh/id_rsa)

4. Enjoy the ride :)

## Limitations
1. SSH Authentication only.

## License
```
MIT License

Copyright (c) 2017 Sector Labs

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
