# Install AWS CLI
1. Download the file using `curl`
```
% curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
```
2. Run the standard macOS installer program
```
% sudo softwareupdate --install-rosetta
% sudo installer -pkg ./AWSCLIV2.pkg -target /
```
3. To verify that the shell can find and run the aws command in your $PATH
```
% which aws
% aws --version
```
---

[back](../command.md)