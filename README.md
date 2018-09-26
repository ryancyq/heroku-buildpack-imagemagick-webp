heroku-buildpack-imagemagick-webp
===========================

Use [ImageMagick](www.imagemagick.org) built with [libwebp](https://code.google.com/p/webp/) inside a Heroku `heroku-16` environment. This buildpack will download both libraries, build them from source and install them along side your app. Your ```$PATH``` and ```$LD_LIBRARY_PATH``` will also be updated to use this version of ImageMagick instead of the default Heroku one, which does _not_ currently have libwebp support.

Since this buildpack is building libwebp and ImageMagick from source your first deploy will take a very long time (around 10 mins). However after building once the installed libraries are stored in a cache directory and will be used for all future deploys. If for any reason you want to force a rebuild of libwebp and ImageMagick, please check out the [heroku-repo](https://github.com/heroku/heroku-repo) plugin.

## Usage

List your buildpacks:
```
heroku buildpacks -a appname
```

Add this buildpack to your app (specify hash to lock down verison):
```
heroku buildpacks:add https://github.com/maximusdominus/heroku-buildpack-imagemagick-webp.git#abc123 -a appname
```


## Verify Installation & Version

You can verify that ImageMagick was built with libwebp support by running the following:

```
heroku run "identify -list format" -a appname
```

If the output includes ```WEBP* rw-   WebP Image Format (libwebp 0.4.2)``` then you're all set.

## Verify the correct ImageMagic version

```
heroku run "identify --version" -a appname
```


If this failed, and you want to [clear the buildpack cache](https://help.heroku.com/18PI5RSY/how-do-i-clear-the-build-cache) to try again:

```
heroku plugins:install heroku-repo -a appname
heroku repo:purge_cache -a appname
```

Then, adjust your buildpacks as necessary and re-run your deploy:

```
git commit --allow-empty -m "Updated buildpacks"
git push origin master
```

## Heroku CI
Add this buildpack to your [app.json](https://devcenter.heroku.com/articles/app-json-schema) file if you're using Heroku's continuous integration / pipelines.

```
{
  "stack": "heroku-16",
  "environments": {
    "test": {
      "buildpacks": [
        { "url": "heroku/ruby" },
        ...
        { "url": "https://github.com/maximusdominus/heroku-buildpack-imagemagick-webp.git#abc123"}
      ],
      "addons": [
        ...
      ],
      "formation": {
        ...
      },
      "scripts": {
        ...
      }
    }
  }
}
```

## Updating The Binary

Only the release versions that end in `-10` are available long-term from `https://www.imagemagick.org/download/releases/`. For example, `6.5.8-10` has been available there for years.

For the most recent versions, it's recommended to source the file elsewhere.


## LICENSE - "MIT License"

Copyright (c) 2014 Nick Jensen, https://nrj.io
Copyright (C) 2012 Heroku, Inc.

Permission is hereby granted, free of charge, to any person
obtaining a copy of this software and associated documentation
files (the "Software"), to deal in the Software without
restriction, including without limitation the rights to use,
copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following
conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.
