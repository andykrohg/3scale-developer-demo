# 3scale Demo: The Good Stuff
### Get 3scale goin
* Create a namespace for 3scale to live, and install the 3scale operator here using OperatorHub
  ```bash
  oc new-project threescale
  ```
* Create a RWO PVC for system storage (otherwise the operator will try to create a RWX one)
  ```bash
  oc apply -f threescale-storagee.yml
  ```
* Create an `APIManager`, being sure to pass the `apps.openshift.example.com` value from your cluster to the  `wildcardDomain` attribute.

* Give the 3scale service account cluster-wide read permissions to enable Service Discovery
  ```bash
  oc adm policy add-cluster-role-to-user view system:serviceaccount:threescale:amp
  ```

### Patch the Content in the Developer Portal
Follow the steps described [here](https://github.com/andykrohg/3scale-discover-APIs/blob/master/doc/apis.json.md)

### Create Integrations
* Deploy Fuse Online using the Operator. All defaults for the `Syndesis` CR will do just fine, though make sure to enable the **Demo Data** option. We're going to create some Integrations here to use with the demo.

#### Create Tasks API
Deploy a pre-built Tasks API Provider using a zip file.  This will leverage a sample Postgres database running in the `syndesis-db` pod.

* Login to Fuse Online using the route, and click **Import** to provide the `TaskAPI-export.zip` file from this repository.

#### Create an Events Webhook
Deploy a webhook. We'll use this to echo out Events that are sent from 3scale to demonstrate integration capabilities with external systems.

* Click **Create Integration**
* Select **Webhook**
* Accept all defaults, and choose **Log** as the final step
* Be sure to check the box next to **Message Body**
* Save and Publish

### Create ActiveDocs
Next, we'll create some ActiveDocs for incorporation in the Developer Portal.

* In 3scale, go to Audience -> Developer Portal -> Active Docs
* Delete the ActiveDocs that already exists for the **Echo API** (since it's OpenAPI 3.0, and we need 2.0 for apis.json compatibility)
* Create new ones using `echo-api-2.0.json` and `tasks-api-2.0.json`