Push to empty repository
------------------------

## Create your repository

Logon your GitLab account, create a new project, (https://gitlab.com/projects/new) and name it `my_sample_use_cases` or a name meaningful to you.  select the setting of 'private'.  **DO NOT** `Initialize with a README`. Select `Create Project`.


## Clone the sample repository

Assume the sample repository URL is https://gitlab.com/automation_consulting/fint/sample_use_cases.

### Set global config values and clone

Open a terminal window in VS Code.  This will be the terminal of your EC2 remote instance.

Use your GitLab username and email address, update the configuration on your remote instance.

```shell
$ git config --global user.name "joelwking"
$ git config --global user.email joel.king@wwt.com
$ git config --list
```
Now clone the repository. If it is marked `private`, at the top of the IDE window, you should be prompted for your GitLab username and password.

```shell 
$ cd ~
$ git clone https://gitlab.com/automation_consulting/fint/sample_use_cases
```
### Change the origin

Now that you have cloned the sample repository, push the sample into your own GitLab account. Substitute your URL for the URL shown in the example.

```shell
$ git remote rename origin old-origin
$ git remote add origin https://gitlab.com/joelwking/my_sample_use_cases.git
$ git push -u origin --all
$ git push -u origin --tags
```

## Save your work

As you make changes, `add`, `commit` and `push` your changes to the remote repository. The next time you launch the lab, you will clone your repository `my_sample_use_cases` rather than the `sample_use_cases`.

By remembering to `push` your changes to your remote repository, your work is saved and your development environment can be re-created from the remote copy.

## Author
Joel W. King @joelwking