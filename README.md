# USAN Enterprise Telephony Gateway

The USAN Dialogflow Enterprise Telephony Gateway provides media server functionality and enterprise telephony (SIP) connectivity between Google Dialogflow Enterprise and your company's enterprise telephony components (PBX or SBC).

Today, you can install and use a free developer edition, which is limited to two concurrent calls. Please check the Marketplace in the near future for the full-capacity production version.

## Prerequisites
Before you begin using the USAN Dialogflow Enterprise Telephony Gateway, you must:

* Install the Google Cloud SDK:
  * https://cloud.google.com/sdk/install


* Set up authentication for Production Applications:
  * https://cloud.google.com/docs/authentication/production

## Installing the USAN Dialogflow Enterprise Telephony Gateway
1. Use the following commands to create the cluster.

        export CLUSTER="<YOUR_CLUSTER_NAME>"
        export ZONE="<GCLOUD_PROJECT_ZONE>"
        gcloud container clusters create "$CLUSTER" --zone "$ZONE" --machine-type n1-standard-1 --num-nodes 2 --scopes=cloud-platform,gke-default


2. Get credentials for the cluster and kubectl.

**Cluster**

        gcloud container clusters get-credentials "$CLUSTER" --zone "$ZONE"

**kubectl**

        kubectl create clusterrolebinding cluster-admin-binding   --clusterrole cluster-admin --user $(gcloud config get-value account)

3. Navigate to the root of the cloned git repository.

4. Install Application CRD.

        make crd/install

5. Export variables.

        export name=dialogflow-telephony-bridge-1
        export namespace=default
        export imageUbbagent=gcr.io/<TBD>:latest
        export imageInit=gcr.io/<TBD>:latest
        export imageTelephonyBridge=gcr.io/<TBD>:latest

6. Create the combined yaml.

        cat manifest/* | envsubst > expanded.yaml

7. Install.

        kubectl apply -f expanded.yaml

## Dialing instructions

You can dial any number at the appliance to reach your DFE agent. To test and verify your audio, dial `sip:test@{svc ip}`.
You should hear test audio: "Hello, world. Hello, world."

## Monitoring the appliance
The appliance reports usage every five minutes.


## Backups
The Gateway appliance does not store any long-term data. Backups are not necessary.

## Upgrades
There is no upgrade path. It is recommended you stand up a new appliance and transition your traffic to the new appliance.

## Troubleshooting

To troubleshoot errors, refer to the container logs for the deployed application. Some common errors:

- No API access. There are two possible reasons:
 - The cluster does not have cloud scope
 - The API is not activated

_Note:_ Launching the application in a VPC is not supported.
