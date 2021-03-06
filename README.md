This respoitory contains releases of Drupal Commerce Kickstart imported from https://www.drupal.org/project/commerce_kickstart

The release versions are on the 'vanilla' branch and each one is tagged as 'vanilla/*DCK version number*'.

The submodule deployment versions with no sites directory are on the 'no_sites' branch and each one is tagged as 'no_sites/*DCK version number*'.

#####To add to a project as a submodule:

* `git submodule add -b no_sites https://github.com/bluerock-solutions/drupal-commerce-kickstart-releases.git drupal`
* `ln -nfsT ../sites drupal/sites`
* `cp -a drupal/example.sites sites`

#####To update Drupal in a project:

* Either this (simple update to latest no_sites but will remove your sites symlink)
    * `git submodule update --remote drupal`
* Or this
    * `cd drupal`
    * `git fetch`
    * `git checkout no_sites` # Can replace no_sites with nosites/x.yz if you want a specific version
    * `cd -`
* Commit update

#####To import a new upstream release:

* `git clone --config core.autocrlf=false THIS_REPO` # Clone and disable autocrlf so line ending stay the same as upstream
* `export vdrupal=7.x-w.yz` # set to next DCK version - eg. 7.x-2.22
* `git checkout vanilla`
* delete (use rm) * .gitignore .htaccess (need to remove all file & dirs EXCEPT .git)
* `ls -A` # should show .git only
* `wget http://ftp.drupal.org/files/projects/commerce_kickstart-$vdrupal-core.tar.gz` # Download next version tar.gz file from https://www.drupal.org/project/commerce_kickstart
* `tar --exclude '.git' --strip-components=1 -xzf commerce_kickstart-$vdrupal-core.tar.gz` # Dump in files from downloaded archive, ensure no .git directories are imported as they confuse git and are unnecessary
* `git add --all`
* `git reset HEAD commerce_kickstart-$vdrupal-core.tar.gz` # Don't commit archive
* `git commit -m "$(echo -e "Import $vdrupal\n\n$(md5sum commerce_kickstart-$vdrupal-core.tar.gz)")"`
* `rm commerce_kickstart-$vdrupal-core.tar.gz` # Remove downloaded archive
* `git tag vanilla/$vdrupal`
* `git checkout no_sites`
* `git merge vanilla/$vdrupal --no-commit`
* Ensure no merge conflicts
* `git diff vanilla/$vdrupal --numstat` # Should show 3 lines added to .gitignore and all sites/* should be renamed to example.sites/*
* `ls -d *sites*` # Should show example.sites and NOT sites
* `git commit --no-edit`
* `git tag no_sites/$vdrupal`
* `git push origin vanilla no_sites vanilla/$vdrupal no_sites/$vdrupal`
