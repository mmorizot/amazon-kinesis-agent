# Port to suse linux 12
Kinesis Agent is a stand-alone Java software application that offers an easy way to collect and send data to Kinesis Data Streams. The agent continuously monitors a set of files and sends new data to your stream. The agent handles file rotation, checkpointing, and retry upon failures. It delivers all of your data in a reliable, timely, and simple manner. It also emits Amazon CloudWatch metrics to help you better monitor and troubleshoot the streaming process.

The procedure described here is designed to install the aws-kinesis-agent in a systemd linux environment. SLES 12 is one such system.

Useful information
Official git repository : https://github.com/awslabs/amazon-kinesis-agent

Axway internal repo : https://git-ext.ecd.axway.int/bds/aws-kinesis-agent

Fork to github to make release opensource: https://github.com/mmorizot/amazon-kinesis-agent

Further details here : 

Amazon documentation : https://docs.aws.amazon.com/streams/latest/dev/writing-with-agents.html

Build from sources
prerequisite
you need :

Java JDK > 1.7
ant 
build
git clone https://git-ext.ecd.axway.int/bds/aws-kinesis-agent
cd aws-kinesis-agent
ant build
# release
tar -zcvf aws-kinesis-agent.tar.gz aws-kinesis-agent
Create an inline AWS 
You need to add an inline AWS policy in the AWS console to allow the gent to communicate with the kinesis server. See  for further details.

install
Here after are the steps necessary in order to install the aws-kinesis-agent in a SLES 12 environment.

1- login as root: 

> sudo su -

2- untar the package:

tar -zxvf aws-kinesis-agent.tar.gz


3- install the agent

> cd Release
> ./setup --install

4- Edit the config file to set the proper parameters

vi /etc/aws-kinesis/agent.json


5- Make sure the files to be monitored are readable by the "aws-kinesis-agent-user:aws-kinesis-agent-user" user

chmod 755 $FILE_TO_BE_MONITORED


6- Make sure the agent starts after a reboot

sudo chkconfig aws-kinesis-agent on


7- Start the agent

sudo service aws-kinesis-agent start


8- CHECKS FOR SANITY:
8.1 check status

service aws-kinesis-agent status
#service should be "active"

8.2 check agent logs

tail -f /var/log/aws-kinesis-agent/aws-kinesis-agent.log
agent should be parsing and sending logs





