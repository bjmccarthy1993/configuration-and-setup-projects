# Set up an SSH Key on your computer to connect with Github
Run the following command to generate a key:

```bash
ssh-keygen -t rsa -b 4096 -C "<your-email-here>" -f ./id_rsa_<your-keyname-here>  
```

You can either add a passphrase when it promps or just hit enter to skip this part.

Then copy the generated files to whereever your OS stores ssh keys. In windows it's:
* ```C:\Users\<user-account-name>\.ssh```



To add the ssh agent to the OpenSSH client running for windows, I had to do the following:

* ssh-agent wasn't running. I figured this out because I tried to run ```ssh-add <path-to-private-key>``` and I had errors.
* I then confirmed OpenSSH was installed by running (in powershell in admin mode) ```Get-Service ssh-agent```, which said the service existed but was stopped.
* I tried running ```Start-Service ssh-agent``` but had errors.
* I then ran the following ```Set-Service -Name ssh-agent -StartupType Automatic``` and then ```Start-Service ssh-agent```, and then confirmed the service was running by re-running ```Get-Service ssh-agent```
* I then ran ```ssh-add <path-to-key>``` where ```<path-to-key>``` was windows filepath based -> ```C:\Users\<username>\.ssh\<key-name>```
* I then copied the public key to github



## Extra setup required for managing multiple github account connections

* When trying to push my code to github, it kept trying to authenticate me as my secondary account.
* I created a file called 'config' in my .ssh directory, which contained this (```<username>``` = my windows username):
```
# Primary Account  (Default)
Host github.com-account-primary
    HostName github.com
    User git
    IdentityFile C:\Users\<username>\.ssh\id_rsa_personal_github
    IdentitiesOnly yes

# Secondary account
Host github.com
    HostName github.com-account-secondary
    User git
    IdentityFile C:\Users\<username>\.ssh\id_ed25519icvGH
    IdentitiesOnly yes
```

* I then had to reset the remote url for my repository by running:
```bash
git remote set-url origin git@github.com-account-primary:<github-username>/<github-repo-name>.git
```

* NOTE: Make sure to set the user.name and user.email appropriately for your local (to the repository) git config.

* I then ran ```ssh -T git@github.com-account-primary``` to check authenticate with github
* Pushing code by using ```git push origin main``` then worked as expected.