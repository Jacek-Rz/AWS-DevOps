## [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/index.html)

### Install AWS CLI under Debian/Ubuntu/Neon
sudo snap install aws-cli --classic

Also useful tool:
sudo apt install jq

### Useful config options
You can find AWS CLI config in file *$HOME/.aws/config* and under section **[default]** you can set many useful options:
- region = eu-west-1 - you can set default region instead of setting it in each command
- cli_auto_prompt = on - you can switch on CLI autocompletion
- cli_history = enabled - you can enable commands history

### Enabling auto-completion
I assume that you install aws-cli from snap and you use bash shell. So you can find aws_completer in this location:
*/snap/aws-cli/current/bin/aws_completer*
Edit your *~/.profile* and add line at the end of the file:
**export PATH=/snap/aws-cli/current/bin/:$PATH**
Edit your *~/.bashrc and add this command at the end of the file:
**complete -C '/snap/aws-cli/current/bin/aws_completer' aws*
Now after logout and login you can use autocompletion with *TAB*.
AWS CLI also has autocompletion under interactive mode. Simply type **aws[ENTER]** and you are in the interactive mode.
Start typing and you will see hints.
