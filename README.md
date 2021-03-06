# deploy_utils
Utilities for deploying projects to EC2

## Table of Contents

* [Documentation](#documentation)
* [Installation](#installation)
* [EC2 Notes](#ec2-notes)
* [Sample AWS Config File](#config-file-data)


## Documentation

For now, just look at the test code.

## Installation 

### In Another Project

Install with pip: `pip install deploy_utils` or put it in your `setup.py` in `install_requires`: `deploy_utils`.

### As Standalone

The project is based of off python 2.7, but is best used with the `virtualenv` development scheme.

1. Install Python 2.7
2. Install virtualenv: `$ [sudo] pip install virtualenv`
3. Instantiate the virtual python environment for the project using python 2.7: 
  - Windows: `virtualenv --python=C:\Python27\python.exe deploy_utils`
  - Linux: `virtualenv -p /path/to/python27 deploy_utils`
4. Browse to project folder `cd deploy_utils`
5. Activate the virtualenv: 
  - Windows: `.\Scripts\activate`
  - Linux: `source bin/activate`
6. (Windows only) Manually install the `pycrypto` library.  The followin command assumes you have 32 bit python 2.7 installed: `pip install http://www.voidspace.org.uk/python/pycrypto-2.6.1/pycrypto-2.6.1-cp27-none-win32.whl`  If 64 bit python 2.7 is installed, run the following command instaed:  `pip install http://www.voidspace.org.uk/python/pycrypto-2.6.1/pycrypto-2.6.1-cp27-none-win_amd64.whl`
7. Install the python project using develop mode: `python setup.py develop`

## EC2 Notes

You will need to do the following for automatically launching Amazon EC2 instances using the scripts:

- Create AWS account
 - Get the access key
 - Get the secret access key
 - Create a profile in `~/.aws/credentials` with the above keys
- Create security group
 - Add your IP to list of allowed inbound traffic [(see aws docs)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html).
- Create key pair [(see aws docs)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html).
 - Download .pem file to computer
- (Windows only) instally PuTTY and PuTTYgen
 - [Download from here](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
 - Create .ppk file [(see aws docs)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html).
- AMI ids
 - You need to use the appropriate one for your region
 - Do this by going to the marketplace and selecting the type to use and then find the AMI id by saying you'll manually launch it.
- Use of CentOS
 - Requires that you agree to some TOS, I did this by launching an instance
 - This library targets CentOS 6 HVM with Updates
 - Pre 2015 instance setup machine with only `root` user
 - Post 2014 uses `centos` user
 - Includes script to automatically install PostGIS.  :)
 
## Config File Data

| Setting Name | Description |
| --- | --- |
| ami_id | The base ami to start from.  See notes about finding AMI ids above. |
| aws_access_key_id | Access key for account. |
| aws_profile_name | The profile name where you put the keys |
| aws_secret_access_key | Secret access key for account. |
| block_device_map | Where to attach storage.  Amazon Linux: `/dev/xvda`  CentOS 6: `/dev/sda1` |
| cron_email | Who to email in cron jobs. |
| key_filename | The filename of your .pem file. |
| key_pair_name | The key pair name for the EC2 instance to use. |
| instance_name | The name to tag the instance with. |
| instance_type | The EC2 instance type.  [(See instance types)](http://aws.amazon.com/ec2/pricing/). |
| non_root_user | The non-root user ot use when fabbing on the machine.  Amazon Linux: `ec2-user`  CentOS 6: `centos` |
| region | The AWS region to connect to. |
| security_groups | Security groups to grant to the instance.  If more than one, seperate with commas. |
| volume_size | Size of the AWS Volume for the new instance in GB. | 
