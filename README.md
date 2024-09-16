# Terraform Beginner Bootcamp 2023






## Install the Terraform CLI

### Considerations with the Terraform CLI changes
The Terraform CLI installation instructions have change due to gpg keyring changes. So we needed refer to the latest install CLI instruction via terraform Documentation and change the scripting for install.

[Install terraform CLI](https://developer.hashicorp.com/well-architected-framework/operational-excellence/verify-hashicorp-binary)

### Consideration for Linux Distribution

This project is built against Ubuntu.
Please consider checking your Linux Distribution and change accordingly to distribution needs. 

[How to check Os Version in Linux OS](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)


Example of checking OS Version:

```sh
$ cat /etc/os-release 
PRETTY_NAME="Ubuntu 22.04.4 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.4 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

### Refactoring into Bash Script

While fixing the Terraform CLI gpg depreciatiuon issues we notice that the bash scripts steps were a considerable amount more code. So we decided to create a bash script to install Terraform CLI. 

This bash script is locate here: [./bin/install_terraform_cli](./bin/install_terraform_cli.sh)

- This wil keep the Gitpod File ([.gitpod.yml](.gitpod.yml)) tidy. 
- This allow us an easier to debug and execute manually Terraform CLI install.
- This will allow better portability for others projects that need to install Terraform CLI.

### Shebang Considerations

A shebang (pronunced Sha-bang) tells the bash script what program that will interpet the script. eg. `#!/bin/bash` 

ChatGPT recommended this format for bash: `#!/ur/bin/env bash`

- For portability for different OS distributions
- will search the user's PATH for the bash executable

https://en.wikipedia.org/wiki/Shebang_%28Unix%29

#### Execution Considerations

When executing the bash Script we can use the `./` shorthand notation to execute the bash script.

eg `./bin/install_terraform_cli`

If wwe are using a script in .gitpod.yml we need to point the script to a program to interpert it.

eg `source ./bin/install_terraform_cli`

#### Linux Permissions Considerations

Linux Permissions works as follows:

In order to make our bash scripts executable we need to change linux permission for the fix to be executable at the user mode.

```sh 
chmod u+x ./bin/install_terraform_cli
``` 
Alternatively:

```sh
chmod 744 ./bin/install_terraform_cli

```

https://en.wikipedia.org/wiki/Chmod

### Github Lifecycle (Before, Init, Command)

We need to be careful when using the Init becaue it will  not rerun if restart an existig workspace.

https://www.gitpod.io/docs/configure/worspaces/tasks