# AWS 2FA Access Keys

## So What Is It?
This is a python script (2.x) that generates temporary AWS authentication keys
for IAM users that have two factor authentication enabled on their access keys.

You should enable two factor authentication on all of your AWS keys where
possible. Otherwise you could be the next
[Code Spaces](http://blog.trendmicro.com/the-code-spaces-nightmare/).

If the access keys for your IAM user account require two factor authentication
you'd need to use the STS service (run the `awscli` tool with the
` sts get-session-token` option ) to get a temporary set of access keys. You
would then need to update your `~/.aws/config` file manually with the temporary
access keys. You would also need to update your `~/.boto` file, if you use the
`boto` module for python.

This script automates this task by taking your AWS username and MFA/two factor
authentication code, getting a temporary set of access keys from the STS
service and updating your `~/.aws/config` (and `~/.boto` file) automatically.

## Installation
To get the script and install the dependencies (requires `pip` to be already be
installed), run the following commands:

```
git clone https://github.com/cdodd/aws-get-token.git
cd aws-get-token
sudo pip install -r requirements.txt
```

You will need to create an AWS config file (`~/.aws/config`) with your AWS key
ID and access key, as follows:

```
[my-keys]
region = eu-west-1
aws_access_key_id = xxxxxxxxxxxxxxxxxxxx
aws_secret_access_key = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

**Note:** Make sure the keep the config section heading as `my-keys`. Also,
update the `region` section to the region you want to connect to.

The last step is to edit the script and edit the `aws_account_id` variable.
You will need to set this to 12 digit account ID where your IAM user resides.

## Generating a Session Token
To generate a new session token, run the command:

`./aws-get-token -u your-username -m 123456`

Replacing `your-username` with your AWS username, and `123456` with your MFA
token. The output should look like the following:

```
Parsing the ~/.aws/config file...                     [OK]
Getting session token from AWS...                     [OK]
Updating the ~/.aws/config file...                    [OK]
Creating/updating the ~/.boto config file...          [OK]
```

Assuming there are no errors, a `default` section will have been created in the
`~/.aws/config` file, which will be used by `awscli`.

The `Credentials` section of your `~/.boto` file will be updated with the new
session details. The `~/.boto` file will be created if it does not already
exist.
