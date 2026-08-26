\# GitHub SSH Troubleshooting



\## Problem



When I attempted to push my local HomeLab repository to GitHub using HTTPS, Git returned the following error:



`git: 'remote-https' is not a git command`



The repository itself was working correctly, but Git could not complete the HTTPS connection to GitHub.



\## Troubleshooting



I verified:



\- Git was installed and working.

\- The local repository was on the `main` branch.

\- The working tree was clean.

\- The GitHub remote named `origin` was configured.

\- The HTTPS error occurred in both PowerShell and Git Bash.



\## Solution



Instead of continuing to troubleshoot the HTTPS helper, I configured SSH authentication.



Steps completed:



1\. Generated an ED25519 SSH key pair.

2\. Added the public SSH key to my GitHub account.

3\. Tested authentication with `ssh -T git@github.com`.

4\. Changed the repository remote from HTTPS to SSH.

5\. Successfully pushed the `main` branch to GitHub.



\## What I Learned



\- A local Git repository exists on my computer.

\- A remote repository exists on GitHub.

\- `origin` is the name assigned to the remote repository.

\- `git push` sends committed changes from the local repository to the remote repository.

\- SSH can securely authenticate my computer with GitHub using a public/private key pair.

\- Troubleshooting should identify which component is failing before making changes.

