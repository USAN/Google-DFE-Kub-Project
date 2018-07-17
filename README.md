# USAN Enterprise Telephony Gateway

The USAN Dialogflow Enterprise Telephony Gateway provides media server functionality and enterprise telephony (SIP) connectivity between Google Dialogflow Enterprise and your company's enterprise telephony components (PBX or SBC).

## Prerequisites
Before you begin using the USAN Dialogflow Enterprise Telephony Gateway, you must:

* Set up a Kubernetes cluster that has appropriate permissions and cloud scope
* Set up kubectl
* Install application resource
* Acquire a license
* Enable the Google Dialogflow API
* Configure an agent to run at Dialogflow

## Using command-line instructions
Use the following commands to set environment variables. Modify if necessary.

    export APP_INSTANCE_NAME=dialogflow-entperise-telephony-gateway-1
    export NAMESPACE=default
    export IMAGE_GATEWAY=launcher.gcr.io/usan-public-209020/dialogflow-entperise-telephony-gateway:1
    export IMAGE_INIT=launcher.gcr.io/usan-public-209020/dialogflow-entperise-telephony-gateway/init:1
    export IMAGE_UBBAGENT=launcher.gcr.io/google/ubbagent  

### Expand manifest template:

    cat manifest/* | envsubst > expanded.yaml

### Run kubectl:

    kubectl apply -f expanded.yaml

_**TO DO:**_ _Fix instructions_

## Dialing instructions

You can dial any number at the appliance to reach your DFE agent. To test and verify your audio, dial `sip:test@{svc ip}`.
You should hear test audio: "Hello, world. Hello, world."

## Monitoring the appliance
The appliance reports usage every five minutes.

## Backups
The Gateway appliance does not store any long-term data. Backups are not necessary.

## Upgrades
There is no upgrade path. It is recommended you stand up a new appliance and transition your traffic to that new appliance.

## Troubleshooting

To troubleshoot errors, refer to the container logs for the deployed application. Some common errors:

- No API access. There are two possible reasons:
 - The cluster does not have cloud scope
 - The API is not activated

Note that launching the application in a VPC is not supported.

_**TO DO:**_ _Add error definitions_
