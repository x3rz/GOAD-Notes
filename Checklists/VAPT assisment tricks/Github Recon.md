## Methodology

1. Curate the list of repo that organisation owns.
Installation of `gh` command:
```bash
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo gpg --dearmor -o /usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh
```


2. Use command `gh repo list <organization-name> | awk '{print$1}' | sed  -e 's|^|https://github.com/|'`.
3. Run these tools on all over them.



### Tools
https://github.com/gwen001/github-search
https://github.com/awslabs/git-secrets
https://github.com/dxa4481/truffleHog
https://github.com/michenriksen/gitrob
https://github.com/eth0izzle/shhgit
https://shhgit.darkport.co.uk/
https://github.com/obheda12/GitDorker
https://github.com/tillson/git-hound
https://github.com/anshumanbh/git-all-secrets
https://github.com/hisxo/gitGraber
https://github.com/auth0/repo-supervisor
https://grep.app

# To-Do
- [ ] Make a single script to use them all at once.