
# Dundas BI

Dundas BI is a state-of-the-art business intelligence platform for data exploration, visual analytics, and creating & sharing dashboards, reports, scorecards, and more. You can deploy Dundas BI as the central data portal for your organization, or integrate it into an existing website as part of a custom or embedded BI solution.

# TL;DR;

```
helm repo add dundas https://helm-charts.dundas.com/repo/
helm install my-dundasbi dundas/dundasbi \
  --set dundas.bi.appDb.appDbConnString="YourConnectionString"
```

# Introduction

This chart bootstraps a Dundas BI deployment on a Kubernetes cluster using the Helm package manager.

This chart has been tested to work with NGINX Ingress.

# Installing the chart

To install the chart with the release name my-dundasbi:

```
helm install my-dundasbi dundas/dundasbi               \
  --set dundas.bi.appDb.express="true"                 \
  --set dundas.bi.key.email="youremail@yourdomain.com" \
  --set dundas.bi.key.key="12345-54321-12345-54321"    
```

> **_NOTE:_**  The email and key are available in the downloads section of the support site: [https://www.dundas.com/support/my-account/#downloads](https://www.dundas.com/support/my-account/#downloads).  If you do not have an account you can create one at [https://www.dundas.com/support/](https://www.dundas.com/support/). It's easy to create and free.

This command deploys Dundas BI on the Kubernetes cluster in the default configuration. The Parameters section lists the parameters that can be configured during installation.

> **_Tip:_**  List all releases using `helm list`

# Uninstalling the chart

```
helm delete my-dundasbi
```
The command removes all the Kubernetes components associated with the chart and deletes the release.  Note if you use the express option the database server and databases will permanently removed when uninstalling the helm chart.

# Parameters
The following tables lists the configurable parameters of the Dundas BI chart and their default values.

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.version**  | The version of Dundas BI images to pull.    | `8.0.0.101` |

# Key

When creating new databases through the chart you will be required to enter your email and key.  The key is available in the downloads section: [https://www.dundas.com/support/my-account/#downloads](https://www.dundas.com/support/my-account/#downloads). If you do not have an account you can create one at [https://www.dundas.com/support/](https://www.dundas.com/support/). It's easy to create and free. If you are not creating new databases these parameters can be left as defaults.

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.key.email**  | The email key required when creating databases.   | `placeholder@placeholder.com` |
| **dundas.bi.key.key**  | The key required when creating databases.   | `00000-00000-00000-00000` |

# Database

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.appDb.express**  | If you choose the express option database pods will be created and used for this instance. You will not have to set any other database values.  This will also create a secret used for the admin account.  | `false` |
| **dundas.bi.appDb.expressimage**  | Postgres image that is used for the express option.    | `postgres:13` |
| **dundas.bi.appDb.appDbConnString**  | The application database connection string.  This is used if `useExistingSecretForAppDbConnString` is set to false.   | `placeholder` |
| **dundas.bi.appDb.warehouseDbConnString**  | Optional - The warehouse database connection string.  If set to empty then the warehouse connection string is discovered from the application database.   | `""` |
| **dundas.bi.appDb.<br />useExistingSecretForAppDbConnString**  | If `useExistingSecretForAppDbConnString` is set to `true` the `appDbConnStringSecretName` and `appDbConnStringSecretKey` are used to set the application database connection string.  Otherwise, the `appDbConnString` is used.   | `false` |
| **dundas.bi.appDb.<br />useExistingSecretForWarehouseDbConnString**  | If `useExistingSecretForWarehouseDbConnString` is set to `true` the `warehouseDbConnStringSecretName` and `warehouseDbConnStringSecretKey` are used to set the warehouse database connection string.  Otherwise, the `warehouseDbConnString` is used.   | `false` |
| **dundas.bi.appDb.<br />secretKeyRef.appDbConnStringSecretName**  | The name of the secret that has the Dundas BI application database connection string.   | `placeholder` |
| **dundas.bi.appDb.<br />secretKeyRef.appDbConnStringSecretKey**  | The key of the secret that has the Dundas BI application database connection string.   | `DBI_CS` |
| **dundas.bi.appDb.<br />secretKeyRef.warehouseDbConnStringSecretName**  | The name of the secret that has the Dundas BI warehouse database connection string.   | `placeholder` |
| **dundas.bi.appDb.<br />secretKeyRef.warehouseDbConnStringSecretKey**  | The key of the secret that has the Dundas BI warehouse database connection string.   | `DBI_WH_CS` |
| **dundas.bi.appDb.appStorage**  | The application database type either `SqlServer` or `Postgres`.   | SqlServer |


# Website
| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.website.port** | The port in the Dundas BI website will be run on. | `8080` |
| **dundas.bi.website.replicaCount** | The number of Dundas BI website replicas.  This is only used if autoscaling.enabled is `false`. | `1` |
| **dundas.bi.website.autoscaling** | This parameters for auto scaling the Dundas BI website.    | `{ enabled: false, minReplicas: 1, maxReplicas: 100, targetCPUUtilizationPercentage: 80 }` |
| **dundas.bi.website.<br />image.override.enabled** | Enables overriding the Dundas BI website image.    | `false` |
| **dundas.bi.website.<br />image.override.name** | The name of the Dundas BI website image to override.    | `` |
| **dundas.bi.website.<br />includeSchedulerAndAuthBridge<br />InWebsiteContainer** | When `true` this will deploy the scheduler and authbridge inside the Dundas BI website container.  If this is done you would not need a separate scheduler deployment and you would set `dundas.bi.scheduler.enabled` to `false`, and `dundas.bi.authbridge.enabled` to `false`.   | `false` |
| **dundas.bi.website.ingress.annotations** | The annotations applied to the website when ingress is enabled.  | `` |
| **dundas.bi.website.kind** | The authbridge kind.  | `Deployment` |
| **dundas.bi.website.service.enabled** | Enable the service.   | `true` |
| **dundas.bi.website.service.type** | The type of service.    | `ClusterIP` |
| **dundas.bi.website.<br />service.sessionAffinity** | The sessionAffinity for the service.    | `ClientIP` |

# Scheduler

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.scheduler.enabled** | This will create the scheduler deployment.  | `true` |
| **dundas.bi.scheduler.replicaCount** | The number of Dundas BI scheduler replicas. | `1` |
| **dundas.bi.scheduler.sleepInSecondsOnStart** | Delay after the website has started to start the scheduler.    | `120` |
| **dundas.bi.scheduler.image.override.enabled** | Enables overriding the Dundas BI scheduler image.    | `false` |
| **dundas.bi.scheduler.image.override.name** | The name of the Dundas BI scheduler image to override.    | `""` |

# AuthBridge

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.authbridge.port** | The port in the Dundas BI authbridge will be run on. | `8080` |
| **dundas.bi.authbridge.replicaCount** | The number of Dundas BI authbridge replicas.  This is only used if dundas.bi.authbridge.autoscaling.enabled is `false`. | `1` |
| **dundas.bi.authbridge.autoscaling** | This parameters for auto scaling the Dundas BI authbridge.    | `{ enabled: false, minReplicas: 1, maxReplicas: 100, targetCPUUtilizationPercentage: 80 }` |
| **dundas.bi.authbridge.<br />ingress.annotations** | The annotations applied to the website when ingress is enabled.  | ``
| **dundas.bi.authbridge.enabled** | This will create the authbridge deployment.  | `false` |
| **dundas.bi.authbridge.service.enabled** | This will create the authbridge service.  | `true` |
| **dundas.bi.authbridge.service.type** | The authbridge service type.  | `ClusterIP` |
| **dundas.bi.authbridge.service.<br />sessionAffinity** | The authbridge service sessionAffinity.  | `ClientIP` |
| **dundas.bi.authbridge.kind** | The authbridge kind.  | `Deployment` |

# Setup

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.setup.image.override.enabled** | Enables overriding the Dundas BI setup image.    | `false` |
| **dundas.bi.setup.image.override.name** | The name of the Dundas BI setup image to override.    | `` |
| **dundas.bi.setup.orchestrator.<br />image.override.enabled** | Enables overriding the Dundas BI setup orchestrator image.    | `false` |
| **dundas.bi.setup.orchestrator.<br />image.override.name** | The name of the Dundas BI setup orchestrator image to override.    | `` |

# Password

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.password.setAdminPassword** | Enables Set admin password when starting container.  This parameter is always `true` when **dundas.bi.appDb.express` is set to `true`. | `false` |
| **dundas.bi.password.<br />useExistingSecretForAdminPassword** | Uses secret to set the admin password.  This will require the `generateRandomPassword` set to `false`. | `false` |
| **dundas.bi.password.secretKeyRef.adminPasswordSecretName** | The secret name to set the admin password. | `false` |
| **dundas.bi.password.secretKeyRef.adminPasswordSecretKey** | The secret key to set the admin password. | `false` |
| **dundas.bi.password.adminPassword** | The admin password is used when `generateRandomPassword`, and `useExistingSecretForAdminPassword` are `false`. | `false` |
| **dundas.bi.password.generateRandomPassword** | Will generate a random password when initializing Dundas BI.  This is done one time and then secret will exist and be reused. | `true` |


# Image 

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **image.pullPolicy** | container image pull policy | `IfNotPresent` |
| **image.PullSecrets** | | `[]`

# Ingress

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **ingress.enabled** | Set to true to enable the ingress.    | `false` |
| **ingress.hosts** | Proxied hosts.    | `[]` |
| **ingress.tls** | This maximum number of Dundas BI websites when autoscaling.    | `[]` |

# Volume 

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.extraVolumes** | Extra Volumes applied to the website, authbridge, scheduler, and setup containers.   | `[]` |
| **dundas.bi.extraVolumeMounts** | Extra Volume Mounts applied to the website, authbridge, scheduler, and setup containers.   | `[]` |
| **dundas.bi.website.extraVolumes** | Mounted extra volumes to the Dundas BI website, Setup containers.  The volume only needs to be added once to be used for both containers.      | `[]` |
| **dundas.bi.website.extraVolumeMounts** | Extra volume mounts for the Dundas BI website container.  | `[]` |
| **dundas.bi.setup.extraVolumes** | Mounted extra volumes to the Dundas BI website, Setup containers.  The volume only needs to be added once to be used for both containers.      | `[]` |
| **dundas.bi.setup.extraVolumeMounts** | Extra volume mounts for the Dundas BI setup image that runs the handle database container init.  These volume mounts are expected to be used with dt calls.    | `[]` |

# Defining the Dundas BI connection

There are three ways to define the Dundas BI connection string:

- Set `dundas.bi.appDb.express` to `true`.  This will create PostgreSQL database pods and will be used for this instance.
- Define the `dundas.bi.appDb.appDbConnString parameter`, and define the `dundas.bi.appDb.appStorage`.
- Create a secret and set `dundas.bi.appDb.useExistingSecretForAppDbConnString` to `true`.  Next pass the name of the secret with `dundas.bi.appDb.secretKeyRef.appDbConnStringSecretName`, and the key with `dundas.bi.appDb.secretKeyRef.appDbConnStringSecretKey`.  Finally, define the `dundas.bi.appDb.appStorage`. 

# Defining the Dundas BI website

The Dundas BI website parameters are defined as parameters that start with `dundas.bi.website`.  The `dundas.bi.website.port` is the `port` defined in the service.   To scale the Dundas BI website you can either set the `dundas.bi.website.replicaCount` to the number of Dundas BI websites you would like across the cluster, or set the `dundas.bi.website.autoscaling` properties which will take precedence over the `dundas.bi.website.replicaCount` property.  When the `dundas.bi.website.autoscaling.enabled` paramter is `true` a HorizontalPodAutoscaler will be created in Kubernetes with the details.

# Defining the Dundas BI scheduler

The Dundas BI scheduler parameters are defined as parameters that start with `dundas.bi.scheduler`.  Dundas BI requires one scheduler service.  If multiple scheduler services are running only one will do the work at a time, and the other will act as a backup if the other goes down.  The Dundas BI Helm chart allows for multiple ways to set up the scheduler service.  It can be added to every website deployment by setting the `dundas.bi.website.includeSchedulerAndAuthBridgeInWebsiteContainer` to true.  This will add the scheduler to the website deployment container(s).  Alternatively, you can run the scheduler as a separate deployment by setting the `dundas.bi.scheduler.enabled` to `true`.

# Defining the Dundas BI authbridge

There are two ways to enable the authbridge.  The first way is to set `dundas.bi.website.includeSchedulerAndAuthBridgeInWebsiteContainer` to `true`.  The second way is set `dundas.bi.website.includeSchedulerAndAuthBridgeInWebsiteContainer` to `false`, and set `dundas.bi.authbridge.enabled` to `true`.  In this case it requires the ingress to be enabled as the authbridge website needs to be placed as a virtual directory under the Dundas BI website.



# Defining the Service

To enable a service to be created with your Dundas BI helm deployment set the `dundas.bi.website.service.enabled` to `true`.  Otherwise it will be expected that after deploying you would set this up manually in Kubernetes.  If you plan on using an ingress the service type is set to the default which is `dundas.bi.website.service.ClusterIP`.  If want to expose the Dundas BI website but don't plan on using an ingress you would use `ClusterIP` and use a kubectl port forward command, or you can expose it by setting the `dundas.bi.website.service.type` to `LoadBalancer`.  These same settings also exist for the authbridge.

# Load Balancing

It is important to enable sticky sessions for any load balanced implementation of Dundas BI.  The following is an example of the annotations to add to enable sticky sessions with a NGINX ingress controller.

```
dundas:
  bi:
    website:
      ingress:
        enabled: true
        annotations: 
          kubernetes.io/ingress.class: nginx
          kubernetes.io/tls-acme: "true"
          nginx.ingress.kubernetes.io/ssl-redirect: "false"
          nginx.ingress.kubernetes.io/affinity: "cookie"
          nginx.ingress.kubernetes.io/session-cookie-name: "dbiroute"
          nginx.ingress.kubernetes.io/session-cookie-expires: "172800"
          nginx.ingress.kubernetes.io/session-cookie-max-age: "172800"
          nginx.ingress.kubernetes.io/session-cookie-path: /

```

# SSL

It is expected to use an ingress to enable SSL with the Dundas BI Helm chart.  You would first create a secret with your certificate information by calling kubectl:

```
kubectl create secret tls exampledomain --key C:\temp\exampledomain.keyfile --cert C:\temp\exampledomain.certfile
```

Then you would apply that secret to the ingress inside the tls:


```
ingress:
  ...
  hosts:
    - host: exampledomain.com
      paths: 
        - /
  tls: [ { hosts: [ exampledomain.com ], secretName: exampledomain } ]
```

# Image Overriding

In order for certain drivers to work it may be necessary to override the Dundas BI docker images.  In this case you would set `dundas.bi.website.image.override.enabled` to `true` and then defining the new image name `dundas.bi.website.image.override.name`.  The Dundas BI website, scheduler, and setup contain these properties and can be overridden.

# Upgrading

An upgrade can be accomplished by setting the `version` parameter to a new version.  Upgrading a major version will require a database upgrade and this will be done automatically when setting the new `version`.  It is recomended to backup the Dundas BI databases before upgrading.
 
# Setup orchestrator

The Dundas BI setup orchestrator provides a means of managing initialization traffic by providing a simple locking mechanism to prevent multiple pods running the initialization at the same time.  It is enabled when `dundas.bi.website.replicaCount` is greater than one, or if `dundas.bi.website.autoscaling.enabled` is set to `true`.

# Resource limits and requests

Each of the following sections allows for setting of Kubernetes limits and requests values:

* `dundas.bi.appDb.expressresources`
* `dundas.bi.website.resources`
* `dundas.bi.website.resources`
* `dundas.bi.scheduler.resources`
* `dundas.bi.authbridge.resources`
* `dundas.bi.authbridge.reverseproxy.resources`
* `dundas.bi.setup.orchestrator.resources`

The initContainers resources will be the same as the pods corresponding limits and resource that it is initializing.


