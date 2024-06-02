# git-help
Git management and troubleshooting

## Troubleshooting

- In case of changing the GitHub `PAT`(Private Access Token) (MacOS env) to keep further access the private repositories need to add the new PAT to `Key chain`
- Update Git credentials. Depending on your OS and how Git was configured, your token may be stored in a credential manager or directly in the repository configuration.
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



Open Terminal.
Check if the file exists in your home directory with ls -la ~. If it’s not visible, you can create it manually.
Use a text editor to open or create this file. You can use nano for this purpose:
bash
Copy code
nano ~/.git-credentials
Enter Your Credentials:

Add a line in the following format:
perl
Copy code
https://<username>:<personal_access_token>@github.com
Replace <username> with your GitHub username and <personal_access_token> with your actual token.
Save the file and exit the editor. In nano, you can do this by pressing Ctrl + X, then pressing Y to confirm, and Enter to save.
Configure Git to Use the store Helper:

If not already done, set Git to use the store credential helper:
bash
Copy code
git config --global credential.helper store
This tells Git to look for credentials in the .git-credentials file.
Test Your Setup:

Try running a Git command that requires authentication, like git fetch or git push, on any of your repositories.
If everything is set up correctly, Git should not prompt for your username or password.
Additional Security Consideration
Storing your credentials in plaintext (as in the .git-credentials file) can pose a security risk, especially if others have access to your computer or if your device is lost or stolen. Here are a couple of alternatives for safer handling:

Use the macOS Keychain: macOS users can use the Keychain to securely store and manage passwords. You can configure Git to use the macOS Keychain by running:

bash
Copy code
git config --global credential.helper osxkeychain
The next time Git prompts you for your username and password, macOS will save these in the Keychain and manage them securely.

Environment Variables: For scripting or development environments, consider using environment variables to store sensitive information temporarily.

By following these steps, you can manually add your credentials to the .git-credentials file and ensure Git uses them for repository access. However, always consider the security implications of storing sensitive information in plaintext.

User
after all settings still having the issue?!

~/.git-credentials:
  https://

~/.gitconfig:
  1 [credential]
  2         helper = store
  3 [safe]
  4         directory = /sources
  5 [user]
  6         name = 
  7         password = 
ChatGPT
Given the configuration you've shared and the persistent issues, let's address a few potential problems and correct any misconfigurations:

Configuration Settings Review

Your .git-credentials and .gitconfig appear to be correctly set up. However, the password entry under [user] in .gitconfig is not standard and will not be used by Git for authentication. This entry can be removed to avoid any confusion.
Remove Unnecessary Password Entry

Open your global .gitconfig file:
bash
Copy code
git config --global --edit
Delete the line that contains password =  or comment it out by adding a # at the beginning of the line.
Save and close the file.
Ensure URL is Correct

Verify that the URL in the .git-credentials file is correct. Make sure there are no typos in the username or token. Also, ensure that the personal access token (token) is still valid and has not been revoked or expired.
Permissions of Personal Access Token

Log into your GitHub account and check the permissions assigned to your personal access token. Ensure it has the appropriate scopes, such as repo, admin:org, user, etc., depending on your needs.
Test Your Git Configuration

Try cloning a repository to see if Git now uses the stored credentials without error:
bash
Copy code
git clone https://github.com/username/your-repository.git
Replace your-repository.git with the actual repository path. This operation should not ask for a username or password if everything is configured correctly.
Check for SSH vs. HTTPS

Ensure that your repository is configured to use HTTPS URLs rather than SSH. The token will not work with SSH URLs. Check this with:
bash
Copy code
git remote -v
If the output shows SSH URLs (git@github.com:...), you need to change them to HTTPS:
bash
Copy code
git remote set-url origin https://github.com/username/your-repository.git
Check for Network Issues

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
| `git clone https://TOKEN@github_url` | e.g. https://token@github.com/username/Ansible-Study.git |
| `git reset --hard origin` | resets the git status to original state |


## Reference:

[Git documentation](https://git-scm.com/doc) - git main documentation
