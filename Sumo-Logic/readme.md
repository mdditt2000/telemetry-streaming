Sumo Logic requires TS declaration and AS3 logger

* Create a local listener in Common

```
{
    "class": "ADC",
    "schemaVersion": "3.10.0",
    "remark": "Example depicting creation of BIG-IP module log profiles",
    "Common": {
        "Shared": {
            "class": "Application",
            "template": "shared",
            "telemetry_local_rule": {
                "class": "iRule",
                "iRule": "when CLIENT_ACCEPTED {\n  node 127.0.0.1 6514\n}"
            },
            "telemetry_local": {
                "class": "Service_TCP",
                "virtualAddresses": [
                    "255.255.255.254"
                ],
                "virtualPort": 6514,
                "iRules": [
                    "telemetry_local_rule"
                ]
            },
            "telemetry": {
                "class": "Pool",
                "members": [
                    {
                        "enable": true,
                        "serverAddresses": [
                            "255.255.255.254"
                        ],
                        "servicePort": 6514
                    }
                ],
                "monitors": [
                    {
                        "bigip": "/Common/tcp"
                    }
                ]
            },
            "telemetry_security_log_profile": {
                "class": "Security_Log_Profile",
                "application": {
                    "localStorage": false,
                    "remoteStorage": "splunk",
                    "protocol": "tcp",
                    "servers": [
                        {
                            "address": "255.255.255.254",
                            "port": "6514"
                        }
                    ],
                    "storageFilter": {
                        "requestType": "all"
                    }
                }
            }
        }
    },
    "VS_tenant": {
        "class": "Tenant",
        "A1": {
            "class": "Application",
            "template": "http",
            "serviceMain": {
                "class": "Service_HTTP",
                "virtualAddresses": [
                    "0.0.0.0"
                ],
                "pool": "web_pool",
                "securityLogProfiles": [
                    {
                        "use": "/Common/Shared/telemetry_security_log_profile"
                    }
                ],
                "policyWAF": {
                    "bigip": "/Common/asmpolicy"
                }
            },
            "web_pool": {
                "class": "Pool",
                "members": [
                    {
                        "servicePort": 80,
                        "serverAddresses": [
                            "172.217.6.110"
                        ]
                    }
                ]
            }
        }
    }
}
```


