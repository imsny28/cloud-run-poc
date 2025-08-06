┌───────────────────────────────────────────────────────────────┐
|           Securing Artifacts using Custom Attestor            |
└───────────────────────────────────────────────────────────────┘
Push Code To Git Repo
---------------------
Open Command prompt and CD into code-root directory
Commands:
    git add .
    git commit -m "lab-22-securing-containers-custom-attestor"
    git push

Enable APIs & Services
-----------------------
Go to APIs & Services
Click on Enable APIs & Services
APIs to Enable:
    Cloud Key Management Service (KMS) API

Create Key Ring
----------------
Go to Security
Click on Key Management
Click on Key Rings
Click on Create Key Ring
    Key Ring Name: cloud-monkey-kms-key-ring
    Location Type: Regional
    Region: us-central1
Click on Create

Create Key
-------------
Go to Security
Click on Key Management
Click on Key Rings
Click on cloud-monkey-kms-key-ring
Click on Create Key
    Key Name: cloud-monkey-kms-key-for-binary-authorization
    Protection Level: Software
    Key material: Generate key
    Purpose: Asymmetric Sign
    Algorithm: Elliptic Curve P-256 SHA256 Digest
Click on Create

Create Custom Attestor
-----------------------
Go to Security
Click on Binary Authorization
Click on Attestors
Click Create Attestor
    Name: cloud-monkey-attestor
    Click Add a PKIX Public Key
    Click Import From Cloud KMS
    Enter key Version Resource Id
        projects/PLACEHOLDER_PROJECT_ID/locations/us-central1/keyRings/cloud-monkey-kms-key-ring/cryptoKeys/cloud-monkey-kms-key-for-binary-authorization/cryptoKeyVersions/1
        Format: projects/KMS_KEY_PROJECT_ID/locations/KMS_KEY_LOCATION/keyRings/KMS_KEYRING_NAME/cryptoKeys/KMS_KEY_NAME/cryptoKeyVersions/KMS_KEY_VERSION
    Click Submit
    Click Done
Click Create

Modify default Policy
---------------------
Go to Security
Click on Binary Authorization
Go to Policy
Click on Edit Policy
Click on Add Attestors
    Select cloud-monkey-attestor
    Click on Add Attestor
Click on Save Policy 

Modify Service Account Permissions
-----------------------------------
Go to IAM & Admin
Click on IAM
Look for sa-cloud-build and click on Edit Principal
Click on Add Another Role
    Roles:
        Cloud KMS CryptoKey Signer/Verifier
        Binary Authorization Attestor Viewer
        Container Analysis Notes Attacher
        Container Analysis Occurrences Editor
        Container Analysis Occurrences for Notes Viewer
Click Save

----------------------


Create Build Trigger From UI
----------------------------
Go to Cloud Build
Click on Triggers
Click on Create Trigger
Name: lab-22-get-customers-svc-build-tr
Region: us-central1
Event: Manual Invocation
Repository Generation: 1st gen
Repository: cloud-monkey-code-repo
Branch: main
Configuration: Cloud Build configuration file (yaml or json)
Cloud Build Configuration File Location: lab-22-securing-containers-custom-attestor/buildconfig/cloudbuild-build.yaml
Service Account: sa-cloud-build-svc


Go to UI and Invoke the Build Trigger
-------------------------------------
Go to Cloud Build
Click on Triggers
Look for trigger named lab-22-get-customers-svc-build-tr and click Run



Create Deploy Trigger From UI
----------------------------
Go to Cloud Build
Click on Triggers
Click on Create Trigger
Name: lab-22-get-customers-svc-deploy-tr
Region: us-central1
Event: Manual Invocation
Repository Generation: 1st gen
Repository: cloud-monkey-code-repo
Branch: main
Configuration: Cloud Build configuration file (yaml or json)
Cloud Build Configuration File Location: lab-22-securing-containers-custom-attestor/buildconfig/cloudbuild-deploy.yaml
Substitution variables
    Click on ADD VARIABLE
        Variable 1:
            _ACCOUNTBALANCE_CLOUD_RUN_SERVICE_URL
        Value: Account balance Cloud Run Service URL

Service Account: sa-cloud-build-svc


Go to UI and Invoke the Build Trigger
-------------------------------------
Go to Cloud Build
Click on Triggers
Look for trigger named lab-22-get-customers-svc-deploy-tr and click Run
