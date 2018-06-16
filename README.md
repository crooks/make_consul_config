This script provides an example of how a consul config can be generated
dynamically from AWS EC2 Metadata.  The templates can be easily modified to
incorporate other configuration options, such as the cloud auto-joining options
in Consul>=0.91 (https://www.consul.io/docs/agent/cloud-auto-join.html).

This script can be run stand-alone to write a config to STDOUT, or from a
systemd service file, such as:

----------
[Unit]
Description=Consul
Wants=network-online.target
After=network-online.target

[Service]
User=consul
Group=consul
Type=simple
ExecStartPre=/usr/local/bin/make_consul_config --file=/etc/consul.d/config.json
ExecStart=/usr/local/bin/consul agent -config-dir=/etc/consul.d
ExecReload=/usr/local/bin/consul reload
KillSignal=SIGINT

[Install]
WantedBy=multi-user.target
----------

Requirements:
python-boto
python-boto3

IAM Policy Requirements:
{
   "Version": "2012-10-17",
   "Statement": [{
      "Effect": "Allow",
      "Action": "ec2:DescribeInstances",
      "Resource": "*"
    }
   ]
}
