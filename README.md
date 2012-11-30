
## Usage
This script should be executed within Jenkins, and will fail otherwise.

First, call the pull request builder, which moves the pull request to your
webroot, and merges it into master.

`pull_request_builder.sh options <webroot>`

Then, call the site cloning script, which uses drush to clone an existing
staging site.

`clone_site.sh options <source-drush-alias> <url>`

## What does it do?
- Moves the checked out repository to a unique directory in the workspace.
- Creates a symlink to the docroot of the drupal directory in the webroot.
- Creates a new branch from the pull request and merges that branch to
  master.
- Copies the settings.php from an existing site to this new drupal site.
- Clones the database from the source site, prefixing any tables with a
  unique identifier to this pull request.
- Rsyncs the files directory from the source site.

## Requirements
- Drush
- A web accessible URL for the pull request. The location of the docroot for
  this URL should be specified with the -l option.
- An existing Drupal 7 site with a site alias, and empty prefix line in the
  database array in settings.php
- A Jenkins job that checks out the Pull Request to 'new_pull_request' directory
  inside the job workspace.

## Arguments
`<webroot>`

  Location of the parent directory of the web root this site will be hosted at.
  Defaults to the job workspace. Note, the Jenkins user must have write
  permissions to this directory.

`<source-drush-alias>`

  The drush alias to the site whose database and files will be cloned to build
  the pull request test environment.

`<url>`

  The parent URL that the destination site will be visible at. Defaults to
  'http://default'. The domain name the site will be set up at. Note, the site
  will be in a subdirectory of this domain using the Pull Request ID, so if the
  Pull Request ID is 234, and you pass https://www.example.com/foo, the site
  will be at https://www.example.com/foo/234.

## Options
* `-h`  Show this message
* `-d`  The path to drush. Defaults to drush.
* `-v`  Verbose mode, passed to all drush commands.
