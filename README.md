# AWS STS Role

This Ansible role allows a user to assume a given role, generating temporary security credentials that can be used to assume the role.

## Requirements

- Python 2.7
- PIP package manager (**easy_install pip**)
- Ansible 2.4 or greater (**pip install ansible**)
- AWS CLI (**pip install awscli**)

## Setup

The recommended approach to use this role is an Ansible Galaxy requirement to your Ansible playbook project.

### Installing using Ansible Galaxy

To set this role up as an Ansible Galaxy requirement, first create a `requirements.yml` file in a subfolder called `roles` and add an entry for this role.  Check the Ansible Galaxy documentation

Note that you can add custom plugin into 'filter_plugin' dir

```
# Example requirements.yml file
- src: https://github.com/aws/Ansible-aws-sts-role.git
  scm: git
  version: v1.0
  name: aws-sts
```

Once you have created `roles/requirements.yml`, you can install the role using the `ansible-galaxy` command line tool.

```
$ ansible-galaxy install --role-file=roles/<rolesfile>.yml --roles-path=<path to roles dir> --force
```
## Usage

### Inputs

The STS role relies on the following inputs:

- `Sts.Role` is Mandatory all other input parameter can be optionally fed from Vars folder

- AWS credentials - you must configure the environment such that your credentials are available to assume the role.

**Important**

- The vars file must adhere to yml format, else you will get error. Please use the debug        option along with pause to check the variable output. Following is my sample vars under       group_vars folder. Don't forget the 3 dashes to mark the file as yml

```
---
# Staging Environment

# Admin Role used for the CF stack. All variable in yml format only
Sts:
  Role: arn:aws:iam::1111111112120:role/AdminAccessforDeploy
```

### Outputs

- `Sts.Credentials.AWS_DEFAULT_REGION`
- `Sts.Credentials.ACCESS_KEY`
- `Sts.Credentials.ACCESS_KEY_ID`
- `Sts.Credentials.AWS_SECRET_KEY`
- `Sts.Credentials.AWS_SECRET_ACCESS_KEY`
- `Sts.Credentials.AWS_SECURITY_TOKEN`

Actual fact output for Sts variable should look like

```
"ansible_facts": {
        "Sts": {
            "Credentials": {
                "AWS_ACCESS_KEY": "AAAAAAAAAAAAAAAAA", 
                "AWS_ACCESS_KEY_ID": "XXXXXXXXXXXXXX", 
                "AWS_DEFAULT_REGION": "us-east-1", 
                "AWS_SECRET_ACCESS_KEY": "YOUJFGokdsdsdsdsweeeCL2SWb5JtSTDOats", 
                "AWS_SECRET_KEY": "YOUJFGokdsdsdsdsweeeCL2SWb5JtSTDOats", 
                "AWS_SECURITY_TOKEN": "FQoDYXdzEBcaDOhIym794RCGWraqLyLwAVbpvRcHaTeL0R5CnlFr6nZu5sfXnHonnfuE4ZM+rtrtjjkjethNuBir7G5z5qot/XNl/QqLUgNfyuPSjHa1OW3wvB+GXC2tWzWc50Uh6//kSRN1SqbbXGgwBhoMGgz6M5Rp7QTtVmgaic9Ka8YvpXunkbqw6vrFeOSHyk/nS+0F5OlVVA18oj0tDp6K8gu7XZNEK+OV1TuwuH9dCKwPab5Avqc/Q9LeGQ69CC3T1xXL/JaH0QLnQFLNIBGu2oiUBZCdDhCyN5uO1kWUxVJlrLn2IB0FgdKmjrdVv95ewU9oyrT+/UsiZ6QjKQiiO393SBQ=="
            }, 
            "Region": "us-east-1", 
            "Role": "arn:aws:iam::1111111112120:role/AdminAccessforDeploy"
        }
    }
```
