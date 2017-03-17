
**Note**: If you are looking for the CSP Status page please proceed to
[https://cyberskills.github.io](https://cyberskills.github.io).

## Running Jekyll

I found the easiest way to run jekyll is to use docker.

```bash
user@host:~/cyberskills.github.io$ cd ../
user@host:~$ docker run -it --rm -v $PWD/cyberskills.github.io/:/site -p 4000:4000 jekyll/jekyll bash
bash-4.3# cd /site
bash-4.3# bundle exec jekyll serve
Configuration file: /site/_config.yml
            Source: /site
       Destination: /site/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.13 seconds.
 Auto-regeneration: enabled for '/site'
Configuration file: /site/_config.yml
    Server address: http://0.0.0.0:4000/
  Server running... press ctrl-c to stop.
```
The auto-regeneration did not work for me, so each change required killing the `jekyll serve` command and starting again.

## Changing the Status

The `_config.yml` file contains the following lines:

```yaml
#This is the section to edit for CSP Status changes
# Change csp_status_severity when the status changes
csp_status_severity: "ok" # this can be either "ok", "minor", "major"
csp_status_message_ok: "All Systems Normal"         # This is not meant to be changed often
csp_status_message_minor: "Some Problems Reported"  # This is not meant to be changed often
csp_status_message_major: "Known Problems"          # This is not meant to be changed often
```

Set `csp_status_severity` to either `ok`, `minor` or `major`.

When you change the severity, the header of the page, `_includes/header.html`, will change the "Current Status" message at the top of the website to include the correct message and class for the span the message is displayed in.

## Adding a New Post

 - Before making a new post, edit the `csp_status_severity` in `_config.yml`.
 - `cd _posts`
 - Copy an existing post
  - `cp 2017-01-27-welcome-to-status.markdown 2017-01-27-my-test-post.markdown`
 - Edit your new file.
  - Set a new title.
  - Set the correct date (and correct timezone)
  - Use markdown for the post body (after the second `---`)


 **Note**: If the date is in the future, jekyll will not render it.

 Preview your new post with the jekyll instruction provided in this file. If everything looks good, commit and push up.



## Replacing Template Defaults

This Jekyll site is using the `minima` template. The files used by `minima` are stored in the gem. If you want to override any of the default files, first, locate the file in the gem, and copy it into the respective folder.

```bash
user@host:~$ docker run -it --rm -v $PWD/cyberskills.github.io/:/site -p 4000:4000 jekyll/jekyll bash
bash-4.3# find / -type d -name '*minima*'
/usr/lib/node_modules/npm/node_modules/node-gyp/node_modules/minimatch
/usr/lib/node_modules/npm/node_modules/node-gyp/node_modules/glob/node_modules/minimatch
/usr/lib/node_modules/npm/node_modules/init-package-json/node_modules/glob/node_modules/minimatch
/usr/lib/node_modules/npm/node_modules/read-package-json/node_modules/glob/node_modules/minimatch
/usr/lib/node_modules/npm/node_modules/glob/node_modules/minimatch
/usr/lib/node_modules/npm/node_modules/fstream-npm/node_modules/fstream-ignore/node_modules/minimatch
/usr/lib/ruby/gems/2.3.0/gems/minima-1.0.1
/usr/lib/ruby/gems/2.3.0/gems/minima-1.0.1/_sass/minima
bash-4.3# cd /usr/lib/ruby/gems/2.3.0/gems/minima-1.0.1/
bash-4.3# ls
CODE_OF_CONDUCT.md  History.markdown    README.md           _includes           _sass               minima.gemspec
Gemfile             LICENSE.txt         Rakefile            _layouts            example
bash-4.3# mkdir /site/_includes/ #if it does not exist
bash-4.3# cp _includes/header.html /site/_includes/header.html
bash-4.3#
```


The two files modified from the default are:

 - `_includes/header.html`
  - Modified to remove navbar and replace it with the status.
 - `_layouts/post.html`
  - Modified to show hour, minute, and timezone information about a post.

