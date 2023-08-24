https://www.howtogeek.com/devops/how-to-set-up-https-personal-access-tokens-for-github-authentication/

`Settings > Developer Settings > Personal Access Token > Generate new token`

> [!blank-container|small]
> ![[images/Pasted image 20220906084842.png]]

![[images/Pasted image 20220906084915.png]]
![[images/Pasted image 20220906084935.png]]

![[images/Pasted image 20220906084955.png]]

Copy the PAT
Tell git to store the credential (will be in `~/.git-credentials`)
`git config credential.helper store`

Push or pull from a repo and when prompted for the password, copy the PAT