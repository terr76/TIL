### S3

**List buckets**

<pre>
aws s3 ls
</pre>

**Bucket location**

<pre>
aws s3api get-bucket-location --bucket &lt;bucket-name&gt;
</pre>

**Logging status**

<pre>
aws s3api get-bucket-logging --bucket &lt;bucket-name&gt;
</pre>
***

### Autoscaling

**Describe autoscale group details and member instances**

<pre>
aws autoscaling describe-auto-scaling-groups \
 --auto-scaling-group-names &lt;as-group-name&gt;
</pre>

***

### CloudFormation

**Template validation**

<pre>
aws cloudformation validate-template \
 --template-body file://myCFN.template.json

aws cloudformation validate-template \
 --template-url https://s3.amazonaws.com/cfn/myCFN.template.json
</pre>

**Listing stacks**

<pre>
aws cloudformation list-stacks \
 --stack-status-filter [ CREATE_COMPLETE | UPDATE_COMPLETE | etc.. ]
</pre>

**Viewing stack events and resources**

<pre>
aws cloudformation describe-stack-events --stack-name &lt;stack-name&gt;

aws cloudformation list-stack-resources --stack-name &lt;stack-name&gt;
</pre>


***

### CloudTrail

**Creating a subscription**

<pre>
aws cloudtrail create-subscription \
 --name cloudtrail-logs-ue1 \
 --s3-use-bucket cloudtrail-logs \
 --s3-prefix stage \
 --sns-new-topic cloudtrail-stage-notify-ue1
</pre>

**Describing and retrieving status**

<pre>
aws cloudtrail describe-trails

aws cloudtrail get-trail-status --name cloudtrail-logs-ue1
</pre>


***

### EC2

**Describing**

<pre>
aws ec2 describe-instances --instance-ids &lt;instance-id&gt;
</pre>

**Starting, stopping, rebooting and killing an instance**

<pre>
aws ec2 start-instances --instance-ids &lt;instance-id&gt;

aws ec2 stop-instances --instance-ids &lt;instance-id&gt;

aws ec2 reboot-instances --instance-ids &lt;instance-id&gt;

aws ec2 terminate-instances --instance-ids &lt;instance-id&gt;
</pre>

**Viewing console output**

<pre>
aws ec2 get-console-output --instance-id &lt;instance-id&gt;
</pre>

**Listing images**

<pre>
aws ec2 describe-images --image-ids &lt;ami-id&gt;
</pre>

**Creating an AMI**

<pre>
aws ec2 create-image \
 --instance-id &lt;instance-id&gt; \
 --name myAMI \
 --description 'Test AMI'
</pre>

**Viewing a security group**

<pre>
aws ec2 describe-security-groups --group-names &lt;group-name&gt;
</pre>

**Checking the enhanced networking attribute**

<pre>
aws ec2 describe-instance-attribute \
 --instance-id &lt;instance-id&gt; \
 --attribute sriovNetSupport
</pre>

***

### VPC

**Describing**

<pre>
aws ec2 describe-vpcs

aws ec2 describe-subnets --filters Name=vpc-id,Values=&lt;vpc-id&gt;

aws ec2 describe-route-tables --filters Name=vpc-id,Values=&lt;vpc-id&gt;

aws ec2 describe-network-acls --filters Name=vpc-id,Values=&lt;vpc-id&gt;

aws ec2 describe-vpc-peering-connections
</pre>


***

### ELB

**Describing**

<pre>
aws elb describe-load-balancers --load-balancer-names &lt;lb-name&gt;

aws elb describe-load-balancer-attributes --load-balancer-name &lt;lb-name&gt;

aws elb describe-load-balancer-policies \
 --policy-names [ &lt;policy-name&gt; | ELBSecurityPolicy-2014-10 ]
</pre>

**Registering and removing instances**

<pre>
aws elb register-instances-with-load-balancer
 --load-balancer-name &lt;lb-name&gt;
 --instances &lt;instance-id&gt;

aws elb deregister-instances-from-load-balancer
 --load-balancer-name &lt;lb-name&gt;
 --instances &lt;instance-id&gt;
</pre>

**Viewing the health of your ELB instances**

<pre>
aws elb describe-instance-health --load-balancer-name &lt;lb-name&gt;
</pre>

***

### IAM

**Uploading a server certificate**

<pre>
aws iam upload-server-certificate
 --server-certificate-name my.cert.com
 --certificate-body file://my.cert.com.crt
 --private-key file://my.cert.com.key
 --certificate-chain file://Verisign_Chain_CA.crt
</pre>

**Listing your certificates**

<pre>
aws iam list-server-certificates
</pre>

***

### Using the "--query" option

(JMESPath query language for JSON)

<b>Describe all instances in a region, or in a specific VPC</b>

<pre><code>
aws ec2 describe-instances \
 --query 'Reservations[*].Instances[*].{Id:InstanceId,Pub:PublicIpAddress,Pri:PrivateIpAddress,State:State.Name}' \
 --output table

aws ec2 describe-instances \
 --filters Name=vpc-id,Values=&lt;vpc-id&gt; \
 --query 'Reservations[*].Instances[*].{Id:InstanceId,Pub:PublicIpAddress,Pri:PrivateIpAddress,State:State.Name}' \
 --output table
</code></pre>

Output:
<pre>
--------------------------------------------------------------
|                      DescribeInstances                     |
+------------+-----------------+------------------+----------+
|     Id     |       Pri       |       Pub        |  State   |
+------------+-----------------+------------------+----------+
|  i-e44ac30e|  10.79.129.62   |  54.172.232.200  |  running |
|  i-68dd7282|  10.79.133.95   |  54.172.204.142  |  running |
|  i-60e5f38d|  10.79.130.54   |  54.172.145.250  |  running |
...
</pre>