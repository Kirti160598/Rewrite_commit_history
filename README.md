

# Rewrite Git History

***Usecase: Remove all files and sub-folders within a given folder from the entire commit history of a git repository, while maintaining the commit structure and branches.***

Motivation: It has been observed that some of the devs have committed creds to the repo, and some have committed their virtual environments to the repo. Now we need to purge all this info from the repository (entire commit history) while preserving the branches and commit history

Remove the following folder paths from the history of https://github.com/django/django
* 		docs/
* 		django/forms/


1. ***Create a fresh clone of your repository, and cd to the clone directory***

    Clone django repo to output_directory
     *git clone https://github.com/django/django.git output_dir
     *cd output_dir


2.***Run a python:3 Docker container interactively, and install git-filter-repo inside it***

           ***run the Docker container***
  
                 docker run --rm -it -v ${PWD}:/app -w /app python:3 /bin/bash

           ****inside the container, install git-filter-repo***
 
           ***Add the backports repo to sources.list***
 
                echo 'deb http://deb.debian.org/debian bullseye-backports main' > /etc/apt/sources.list.d/backports.list

           ***Update the list of available packages***
                apt-get update

           ***Install git-filter-repo, adding the required /bullseye-backports suffix
                apt-get install -y git-filter-repo/bullseye-backports



3. ***Run the git-filter-repo commands to move all the files to the engine subdirectory, and then move the docs and django/forms files back***

     Move everything to the engine/ subfolder
          
          git filter-repo --replace-refs delete-no-add --to-subdirectory-filter engine/

    Move docs and django/forms back to the root
     
          git filter-repo --path-rename engine/docs:docs
          git filter-repo --path-rename engine/django/forms:django/forms

 

4. Check history has been deleted 
       git log -1 <commit-id>

Note: engine folder has all the data related to task.
