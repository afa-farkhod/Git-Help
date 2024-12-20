# git-help
Git management and troubleshooting

## Troubleshooting

- In case of changing the GitHub `PAT`(Private Access Token) (MacOS env) to keep further access the private repositories need to add the new PAT to `Key chain`
- Update Git credentials. Depending on your OS and how Git was configured your token may be stored in a credential manager or directly in the repository configuration.
  - For macOS:
    - The macOS Keychain usually manages GitHub credentials. You can update your token here:
    - Open Keychain Access.
    - Search for github.com.
    - Find the entry (it should be a web form password or Internet password).
    - Update the password field with your new token.
  - For Windows:
    - Credentials are typically managed by the Windows Credential Manager.
    - Open Credential Manager from the Control Panel.
    - Go to Windows Credentials.
    - Find the entry for `git:https://github.com`
    - Update the entry with your new token.
  - For Linux:
    - If you're using a credential helper, such as git-credential-store or git-credential-cache, you can update your credentials via command line:
      - `git config --global --unset credential.helper`
      - `git config --global credential.helper store`
- When you get the following type of error log:
```
git config --global --unset credential.helper                   
warning: credential.helper has multiple values
```
  - The warning `credential.helper` has multiple values indicates that multiple credential helpers have been set in your Git configuration. This can happen if you've configured Git to use different credential helpers at different levels (local, global, system) or multiple entries were added to the global configuration.
  - To resolve this and ensure your new personal access token is used, you might want to clear all credential helper settings and then reconfigure one that will use your new token. Here's how you can do that:
    - Clearing Multiple Credential Helpers
    - List Current Configurations
    - First, check what credential helpers are configured:
```
git config --global --get-all credential.helper
```
  - Remove Each Credential Helper
    - If the above command lists multiple helpers, you'll need to unset each one individually. You can do this by repeating the unset command for each helper listed:
```
git config --global --unset credential.helper
```
  - Set a New Credential Helper
    - After clearing the old settings, set a new credential helper that will prompt you to enter your credentials, which can then be saved(cache is a simple helper that temporarily stores credentials in memory for a short time):
```
git config --global credential.helper cache
```
- While running this command: `git config --global --get-all credential.helper` , and get the following output:
```
store
store
```
  - The output you see, with two store entries for the credential.helper, indicates that the store credential helper has been set multiple times in your global Git configuration. To remove these duplicate entries, you'll need to use the --unset command for each instance that appears.
  - Here’s how you can remove the duplicated store entries:
  - Since you have two store entries, you need to run the unset command twice to remove each one(run the --unset command as many times as needed until all duplicate entries are removed. Each execution of the command removes one instance of the configuration):
```
git config --global --unset credential.helper
git config --global --unset credential.helper
```
  - Once the duplicates are removed, you can reconfigure a single credential helper. Here are a couple of options:
    - Use store to save credentials on disk:
```
git config --global credential.helper store
```
- After configuring the credential helper, the next time you interact with GitHub (via commands like git push or git pull), you should be prompted to enter your username and password. Use your GitHub username and your new personal access token as the password. This information will then be stored according to the method you set with the credential.helper configuration.

- MacOS `Keychain`: macOS users can use the Keychain to securely store and manage passwords. 
--------------------------------------------------------------------------------------------------------

- What is GitHub gist?
  - Gists provide a simple way to share code snippets with others. Every gist is a Git repository, which means that it can be forked and cloned [Guide Link](https://docs.github.com/en/get-started/writing-on-github/editing-and-sharing-content-with-gists/creating-gists).
  - If you are signed in to GitHub when you create a gist, the gist will be associated with your account and you will see it in your list of gists when you navigate to your [gist home page](https://gist.github.com/)

--------------------------------------------------------------------------------------------------------

## Git commands

| Command | Description |
| --- | --- |
| `git checkout -b <branch_name>` | used to change to given branch |
| `git add .` | used to add modified changes to branch | 
| `git commit -m "your commit message"` | used to commit to modified branch |
| `git push origin <branch_name>` | used to push branch to git |
| `git tag -f -a test-tag-name -m "test-tag-name-case"` | used to tag the particular branch with comment |
| `git push origin --tags -f` | push tag to git |
| `git checkout -b <branch_name>` | creates new branch based on master branch |
| `git clean -fdx` | cleans git credential cache |
| `git config --global credential.helper cache` | cleans the git cache in local |
| `git config --global credential.helper 'cache --timeout=3600'` | set the local cache cleaning time |
| `git cherry-pick <commit-hash>` | pick and choose individual commits and apply them to a different branch, rather than merging entire branches |
| `git clone https://TOKEN@github_url` | e.g. `https://token@github.com/username/Ansible-Study.git` |
| `git reset --hard origin` | resets the git status to original state |


## Reference:

[Git documentation](https://git-scm.com/doc) - git main documentation

[GitHub CLI](https://github.com/cli/cli) - `gh` is GitHub on the command line. It brings pull requests, issues, and other GitHub concepts to the terminal next to where you are already working with git and your code.
