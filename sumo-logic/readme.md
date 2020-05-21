# Sumo Logic requires TS declaration and AS3 logger

* Create a local listener in Common

No AFM provisioned on BIG-IP so removed Security_Log_Profile with networking options. Add the below network configuration for AFM data
```
"network": {
                "publisher": {
                    "use": "telemetry_publisher"
                },
                "logRuleMatchAccepts": false,
                "logRuleMatchRejects": true,
                "logRuleMatchDrops": true,
                "logIpErrors": true,
                "logTcpErrors": true,
                "logTcpEvents": true
                }
            }
```