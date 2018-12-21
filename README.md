# USAN Dialogflow Enterprise Telephony Gateway

The USAN Dialogflow Enterprise Telephony Gateway allows Google Dialogflow Enterprise Edition users to easily connect to their projects from existing phone numbers and enterprise telephony networks. The solution connects to corporate PBXs and SBCs via SIP/RTP and provides a bridging gateway function into the Google Dialogflow Enterprise voice API. The solution provides the necessary services to provide optional voice activity detection and audio end-pointing before sending audio to Google Dialog Flow for speech recognition, forwarding the voice utterances to Dialogflow Enterprise, enabling Google WaveNet text-to-speech from Google DialogFlow Enterprise to the RTP stream, and managing the call transfers back to the Corporate Telephony network via SIP. This edition supports 49 concurrent sessions per deployed gateway. More sessions per gateway are possible if custom GCP firewall configurations are created – contact USAN for information on how.

## Prerequisites
Before you begin using the USAN Dialogflow Enterprise Telephony Gateway, you must:

* Install the Google Cloud SDK:
  * https://cloud.google.com/sdk/install


* Set up authentication for Production Applications:
  * https://cloud.google.com/docs/authentication/production


* Create a new Google Cloud Platform project:
  * https://cloud.google.com/resource-manager/docs/creating-managing-projects


* Set up Dialogflow:
  * https://dialogflow.com/docs/getting-started

## Installing the USAN Dialogflow Enterprise Telephony Gateway
This two-minute video walks you through the initial steps of installing the USAN Dialogflow Enterprise Telephony Gateway:

[![Installing the USAN Dialogflow Enterprise Telephony Gateway](https://i.imgur.com/6jP9ahA.jpg)](http://www.youtube.com/watch?v=Z--sN2gD9ng "Installing the USAN Dialogflow Enterprise Telephony Gateway")

## Using the command line to install the USAN Dialogflow Enterprise Telephony Gateway
You can also install the USAN Dialogflow Enterprise Telephony Gateway from the command line.

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
        export imageTelephonyBridge=marketplace.gcr.io/usan-gcp/u-dfe-tg:1


6. Create the combined yaml.

        cat manifest/* | envsubst > expanded.yaml

7. Install.

        kubectl apply -f expanded.yaml

8. Follow the post-installation instructions that are displayed in the application panel.


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
