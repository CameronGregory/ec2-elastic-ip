ec2-elastic-ip
==============

Init script for ubuntu that will set the ip address and hostname of an amazon spot instance using user-data.

##Quick Start

```bash
sudo pip install awscli
sudo cp ec2-elastic-ip /etc/init.d/
sudo chmod 755 /etc/init.d/ec2-elastic-ip
sudo update-rc.d ec2-elastic-ip defaults
sudo update-rc.d ec2-elastic-ip enable
sudo vi /etc/default/ec2-elastic-ip
```
The contents of /etc/default/ec2-elastic-ip should be:
```bash
export AWS_ACCESS_KEY_ID="xxx"
export AWS_SECRET_ACCESS_KEY="xxx/yyy"
```
Set the user-data on the spot instance like this:
```code
ip=1.2.3.4&hostname=aws1.example.com
```
I use a script to kick off the persistent spot instance:
```bash
MY_AMI="ami-123451"
REGION="us-west-2a"
AWS_IP="1.2.3.4"
AWS_HOSTNAME="aws.example.com"
USER_DATA="ip=$AWS_IP&hostname=$AWS_HOSTNAME"
USER_DATA_B64=`echo $AWS1_USER_DATA | base64 -w0`

aws ec2 request-spot-instances --spot-price "0.10" --instance-count 1 --type "persistent" --launch-specification "{\"ImageId\":\"$MY_AMI\",\"InstanceType\": \"r3.2xlarge\",\"UserData\":\"$AWS_USER_DATA_B64\",\"Placement\":{\"AvailabilityZone\":\"$REGION\"}}"
```

Of course you can add additional parameters that would be ignored.
