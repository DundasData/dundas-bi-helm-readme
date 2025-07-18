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
| **dundas.bi.version**  | The version of Dundas BI images to pull.    | `{CurrentMajorVersion}.{CurrentMinorVersion}` |

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
| **dundas.bi.appDb.expressstoragesize**  | The size of the postgres pv.    | `10Gi` |
| **dundas.bi.appDb.appDbConnString**  | The application database connection string.  This is used if `useExistingSecretForAppDbConnString` is set to false.   | `placeholder` |
| **dundas.bi.appDb.warehouseDbConnString**  | Optional - The warehouse database connection string.  If set to empty then the warehouse connection string is discovered from the application database.   | `""` |
| **dundas.bi.appDb.<br />useExistingSecretForAppDbConnString**  | If `useExistingSecretForAppDbConnString` is set to `true` the `appDbConnStringSecretName` and `appDbConnStringSecretKey` are used to set the application database connection string.  Otherwise, the `appDbConnString` is used.   | `false` |
| **dundas.bi.appDb.<br />useExistingSecretForWarehouseDbConnString**  | If `useExistingSecretForWarehouseDbConnString` is set to `true` the `warehouseDbConnStringSecretName` and `warehouseDbConnStringSecretKey` are used to set the warehouse database connection string.  Otherwise, the `warehouseDbConnString` is used.   | `false` |
| **dundas.bi.appDb.<br />secretKeyRef.appDbConnStringSecretName**  | The name of the secret that has the Dundas BI application database connection string.   | `placeholder` |
| **dundas.bi.appDb.<br />secretKeyRef.appDbConnStringSecretKey**  | The key of the secret that has the Dundas BI application database connection string.   | `DBI_CS` |
| **dundas.bi.appDb.<br />secretKeyRef.warehouseDbConnStringSecretName**  | The name of the secret that has the Dundas BI warehouse database connection string.   | `placeholder` |
| **dundas.bi.appDb.<br />secretKeyRef.warehouseDbConnStringSecretKey**  | The key of the secret that has the Dundas BI warehouse database connection string.   | `DBI_WH_CS` |
|**dundas.bi.appDb.useExistingSecretForAppDbCreatorConnectionString**| If `useExistingSecretForAppDbConnString` is set to `true` the `appDbCreatorConnStringSecretName` and `appDbCreatorConnStringSecretKey` are used to set the creator application database connection string. This is optional.  |`false`|
|**dundas.bi.appDb.useExistingSecretForWhDbCreatorConnectionString**| If `useExistingSecretForAppDbConnString` is set to `true` the `warehouseDbCreatorConnStringSecretName` and `warehouseDbCreatorConnStringSecretKey` are used to set the creator warehouse database connection string.  This is optional. |`false`|
|**dundas.bi.appDb.<br />secretKeyRef.appDbCreatorConnStringSecretName**| The name of the secret that has the Dundas BI application database creator connection string. This is optional.  |`placeholder`|
|**dundas.bi.appDb.<br />secretKeyRef.appDbCreatorConnStringSecretKey**| The key of the secret that has the Dundas BI application database creator connection string. This is optional.  |`DBI_CREATOR_CONNECTION_STRING`|
|**dundas.bi.appDb.<br />secretKeyRef.warehouseDbCreatorConnStringSecretName**| The name of the secret that has the Dundas BI warehouse database creator connection string. This is optional.  |`placeholder`|
|**dundas.bi.appDb.<br />secretKeyRef.warehouseDbCreatorConnStringSecretKey**| The key of the secret that has the Dundas BI warehouse database creator connection string. This is optional.  |`DBI_WAREHOUSE_CREATOR_CONNECTION_STRING`|
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
| **dundas.bi.website.<br />service.sessionAffinity** | The sessionAffinity for the service.    | `None` |
| **dundas.bi.website.useFullImage** | Determines if the full website image incorporating other services together is used as described in later sections. | `false` |

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

# GatewayHub

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.gatewayhub.port** | The port in the Dundas BI gatewayhub will be run on. | `8080` |
| **dundas.bi.gatewayhub.<br />ingress.annotations** | The annotations applied to the website when ingress is enabled.  | ``
| **dundas.bi.gatewayhub.enabled** | This will create the gatewayhub deployment.  | `false` |
| **dundas.bi.gatewayhub.service.enabled** | This will create the gatewayhub service.  | `true` |
| **dundas.bi.gatewayhub.service.type** | The gatewayhub service type.  | `ClusterIP` |
| **dundas.bi.gatewayhub.kind** | The gatewayhub kind.  | `Deployment` |

# LogReceiver

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.logreceiver.port** | The port the Dundas BI LogReceiver will run on. | `8080` |
| **dundas.bi.logreceiver.enabled** | This will create the LogReceiver deployment. | `false` |
| **dundas.bi.logreceiver.autoscaling** | This parameters for auto scaling the Dundas BI LogReceiver. | `{ enabled: false, minReplicas: 1, maxReplicas: 100, targetCPUUtilizationPercentage: 80 }` |
| **dundas.bi.logreceiver.service.enabled** | This will create the LogReceiver service. | `true` |
| **dundas.bi.logreceiver.service.type** | The LogReceiver service type. | `ClusterIP` |
| **dundas.bi.logreceiver.kind** | The LogReceiver kind. | `Deployment` |

# Python

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.python.autoscaling** | This parameter is for auto scaling the Dundas BI Python website. | `{ enabled: false, minReplicas: 1, maxReplicas: 100, targetCPUUtilizationPercentage: 80 }` |
| **dundas.bi.python.enabled** | This will create the Python deployment. | `true` |
| **dundas.bi.python.kind** | The Python kind. | `Deployment` |
| **dundas.bi.python.maxReceiveMessageSize** | The maximum message size in MB that the Python service will accept. If empty, the default is used. | `` |
| **dundas.bi.python.port** | The port the Dundas BI Python website will be run on. | `8080` |
| **dundas.bi.python.service.enabled** | This will create the Python service. | `true` |
| **dundas.bi.python.service.type** | The Python service type. | `ClusterIP` |
| **dundas.bi.python.setup.pythonModules** | List of Python modules to install, specified as a space-separated list (e.g. "SciPy==1.15.2 TensorFlow"). | `` |

# Export
| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.export.port** | The port in the Dundas BI export website will be run on. | `8080` |
| **dundas.bi.export.enabled** | This will create the export deployment.  | `true` |
| **dundas.bi.export.autoscaling** | This parameters for auto scaling the Dundas BI export website.    | `{ enabled: false, minReplicas: 1, maxReplicas: 100, targetCPUUtilizationPercentage: 80 }` |
| **dundas.bi.export.service.enabled** | This will create the export service.  | `true` |
| **dundas.bi.export.service.type** | The export service type.  | `ClusterIP` |
| **dundas.bi.export.kind** | The export kind.  | `Deployment` |

# Storage
| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.storage.port** | The port in the Dundas BI storage website will be run on. | `8080` |
| **dundas.bi.storage.enabled** | This will create the storage deployment.  | `true` |
| **dundas.bi.storage.autoscaling** | This parameters for auto scaling the Dundas BI storage website.    | `{ enabled: false, minReplicas: 1, maxReplicas: 100, targetCPUUtilizationPercentage: 80 }` |
| **dundas.bi.storage.service.enabled** | This will create the storage service.  | `true` |
| **dundas.bi.storage.service.type** | The storage service type.  | `ClusterIP` |
| **dundas.bi.storage.kind** | The storage kind.  | `Deployment` |

# Setup

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.setup.handledatabase.<br />continueOnError** | This controls whether the Dundas BI website's database initialization container (handledatabase) should continue running despite encountering errors.  Starting with version 25.1 this will be defaulted to false.    | `false` |
| **dundas.bi.setup.image.override.enabled** | Enables overriding the Dundas BI setup image.    | `false` |
| **dundas.bi.setup.image.override.name** | The name of the Dundas BI setup image to override.    | `` |
| **dundas.bi.setup.extraEnvs** | Extra environment variables added to the Dundas BI Setup (HandleDatabase) container.    | `` |
| **dundas.bi.setup.orchestrator.<br />image.override.enabled** | Enables overriding the Dundas BI setup orchestrator image.    | `false` |
| **dundas.bi.setup.orchestrator.<br />image.override.name** | The name of the Dundas BI setup orchestrator image to override.    | `` |

# DT

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.dt.setup.calls.<br />afterDatabaseCreation** |  DT calls that occur after the database is created.  This will only happen one time.    | `[]` |
| **dundas.bi.dt.setup.calls.<br />onLoad** | DT calls that occur at the end of the handledatabase container init every time.    | `[]` |


# Rabbit MQ

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.rabbitmq.enabled** |  # Enables the Rabbit MQ Broadcast service.   | `false` |
| **dundas.bi.rabbitmq.kind**  | The Rabbit MQ deployment kind.    | `Deployment` |
| **dundas.bi.rabbitmq.rabbitmqimage**  | The Rabbit MQ image name.    | `rabbitmq:3` |
| **dundas.bi.rabbitmq.exchangename**  | The Rabbit MQ exchange name.    | `dundasbi` |
| **dundas.bi.rabbitmq.storagesize**  | The size of the rabbitmq storage.    | `1Gi` |

# Password

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.password.enforceDefaultPasswordPolicyAtHelm** | This will enforce the default password policy at the helm level when deploying. | `true` |
| **dundas.bi.password.setAdminPassword** | Enables Set admin password when starting container.  This parameter is always `true` when `dundas.bi.appDb.express` is set to `true`. | `false` |
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
| **dundas.bi.python.extraVolumes** | The extra volumes to the Dundas BI python containers.    | `[]` |
| **dundas.bi.python.extraVolumeMounts** | The extra volume mounts for the python containers.   | `[]` |
| **dundas.bi.export.extraVolumes** | The extra volumes to the Dundas BI export containers.       | `[]` |
| **dundas.bi.export.extraVolumeMounts** | The extra volume mounts for the export containers.     | `[]` |
| **dundas.bi.storage.extraVolumes** | The extra volumes to the Dundas BI storage containers.     | `[]` |
| **dundas.bi.storage.extraVolumeMounts** | The extra volume mounts for the storage containers.    | `[]` |
| **dundas.bi.storageClassName** | The storage class used for volumes mounted for Dundas BI deployments.  This is optional and will not specify a class if one is not set.  | `` |

# Sidecars and InitContainers

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.website.initContainers** | The init container added to the Dundas BI website.      | `[]` |
| **dundas.bi.website.sidecars** | The sidecar container added to the Dundas BI website.  | `[]` |
| **dundas.bi.scheduler.initContainers** | The init container added to the Dundas BI scheduler.      | `[]` |
| **dundas.bi.scheduler.sidecars** | The sidecar container added to the Dundas BI scheduler.  | `[]` |
| **dundas.bi.authbridge.initContainers** | The init container added to the Dundas BI authbridge.      | `[]` |
| **dundas.bi.authbridge.sidecars** | The sidecar container added to the Dundas BI authbridge.  | `[]` |
| **dundas.bi.gatewayhub.initContainers** | The init container added to the Dundas BI gatewayhub.      | `[]` |
| **dundas.bi.gatewayhub.sidecars** | The sidecar container added to the Dundas BI gatewayhub.  | `[]` |
| **dundas.bi.export.initContainers** | The init container added to the Dundas BI export.      | `[]` |
| **dundas.bi.export.sidecars** | The sidecar container added to the Dundas BI export.  | `[]` |
| **dundas.bi.python.initContainers** | The init container added to the Dundas BI python.      | `[]` |
| **dundas.bi.python.sidecars** | The sidecar container added to the Dundas BI python.  | `[]` |
| **dundas.bi.storage.initContainers** | The init container added to the Dundas BI storage.      | `[]` |
| **dundas.bi.storage.sidecars** | The sidecar container added to the Dundas BI storage.  | `[]` |

# Microservice

| Parameters | Description | Default |
| ---------- | ----------- | ------- |
| **dundas.bi.microservice.initialLoggingVerbosity** | The initial log level for the EDC (Will always use this log level), Storage (Will use this log level until a build request), Python (Will use this log level until the first python request), and Export (Will use this log level until the first export request) microservices.  

# Defining the Dundas BI connection

There are three ways to define the Dundas BI connection string:

- Set `dundas.bi.appDb.express` to `true`.  This will create PostgreSQL database pods and will be used for this instance.
- Define the `dundas.bi.appDb.appDbConnString parameter`, and define the `dundas.bi.appDb.appStorage`.
- Create a secret and set `dundas.bi.appDb.useExistingSecretForAppDbConnString` to `true`.  Next pass the name of the secret with `dundas.bi.appDb.secretKeyRef.appDbConnStringSecretName`, and the key with `dundas.bi.appDb.secretKeyRef.appDbConnStringSecretKey`.  Finally, define the `dundas.bi.appDb.appStorage`. 

# Defining the Dundas BI website

The Dundas BI website parameters are defined as parameters that start with `dundas.bi.website`.  The `dundas.bi.website.port` is the `port` defined in the service.   To scale the Dundas BI website you can either set the `dundas.bi.website.replicaCount` to the number of Dundas BI websites you would like across the cluster, or set the `dundas.bi.website.autoscaling` properties which will take precedence over the `dundas.bi.website.replicaCount` property.  When the `dundas.bi.website.autoscaling.enabled` parameter is `true` a HorizontalPodAutoscaler will be created in Kubernetes with the details.

# Defining the Dundas BI scheduler

The Dundas BI scheduler parameters are defined as parameters that start with `dundas.bi.scheduler`.  Dundas BI requires one scheduler service.  If multiple scheduler services are running only one will do the work at a time, and the other will act as a backup if the other goes down.  The Dundas BI Helm chart allows for multiple ways to set up the scheduler service.  It can be added to every website deployment by setting the `dundas.bi.website.useFullImage` to true.  This will add the scheduler to the website deployment container(s).  Alternatively, you can run the scheduler as a separate deployment by setting the `dundas.bi.scheduler.enabled` to `true`.

# Defining the Dundas BI authbridge

There are two ways to enable the authbridge.  The first way is to set `dundas.bi.website.useFullImage` to `true`.  The second way is set `dundas.bi.website.useFullImage` to `false`, and set `dundas.bi.authbridge.enabled` to `true`.  In this case it requires the ingress to be enabled as the authbridge website needs to be placed as a virtual directory under the Dundas BI website.

# Defining the Dundas BI export

There are two ways to enable the export.  The first way is to set `dundas.bi.website.useFullImage` to `true`.  The second way is set `dundas.bi.website.useFullImage` to `false`, and set `dundas.bi.export.enabled` to `true`.  

# Defining the Dundas BI python

There are two ways to enable the python.  The first way is to set `dundas.bi.website.useFullImage` to `true`.  The second way is set `dundas.bi.website.useFullImage` to `false`, and set `dundas.bi.python.enabled` to `true`.  

# Defining the Dundas BI Storage

The Dundas BI Storage service is enabled by setting `dundas.bi.storage.enabled` to `true`.  This service will move the building of warehoused and in-memory cubes into its own service, and away from the Dundas BI website.

# Defining the Service

To enable a service to be created with your Dundas BI helm deployment set the `dundas.bi.website.service.enabled` to `true`.  Otherwise it will be expected that after deploying you would set this up manually in Kubernetes.  If you plan on using an ingress the service type is set to the default which is `dundas.bi.website.service.ClusterIP`.  If want to expose the Dundas BI website but don't plan on using an ingress you would use `ClusterIP` and use a kubectl port forward command, or you can expose it by setting the `dundas.bi.website.service.type` to `LoadBalancer`.  These same settings also exist for the authbridge.

# Session Affinity and Caching

Dundas BI requires sticky sessions (session affinity) for correct operation. Sticky sessions ensure that a user's requests are consistently directed to the same server instance. Configure sticky sessions according to your ingress provider's documentation. For NGINX ingress, use the provided annotations example.

A distributed cache using Redis is available (`dundas.bi.cache.redis.enabled: true`), but has known issues and is not recommended. Sticky sessions are the supported approach.

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

An upgrade can be accomplished by setting the `version` parameter to a new version.  Upgrading a major version will require a database upgrade and this will be done automatically when setting the new `version`.  It is recommended to backup the Dundas BI databases before upgrading.
 
# Setup orchestrator

The Dundas BI setup orchestrator provides a means of managing initialization traffic by providing a simple locking mechanism to prevent multiple pods running the initialization at the same time.  It is enabled when `dundas.bi.website.replicaCount` is greater than one, or if `dundas.bi.website.autoscaling.enabled` is set to `true`.

# DT setup calls

To call the Dundas BI dt utility during the setup process, use dt setup calls.  The following example adds three dundas bi dt calls, two that occur after database is created (1. Sets a Dundas BI configuration property, 2. Imports a dbie file from a mounted volume), and one that occurs on load (Runs a healthcheck):  

```
dundas:
  bi:
    website:
      extraVolumes:
        - name: my-storage
          persistentVolumeClaim:
            claimName: scriptlibrary
      extraVolumeMounts: 
        - name: my-storage
          mountPath: /tmp/scriptlibrary

    dt:
      setup:
        calls:
          onLoad:
            - "healthcheck" 
          afterDatabaseCreation:
            - "setConfigValue /settingId:\"a0b50536-a9cb-444d-a5d6-330c6dc2dc07\" /value:\"0.0.0.0-255.255.255.255\""             
            - "import /dbie:\"/tmp/scriptlibrary/DundasScriptLibrary.dbie\""
```

# Resource limits and requests

Each of the following sections allows for setting of Kubernetes limits and requests values:

* `dundas.bi.appDb.expressresources`
* `dundas.bi.website.resources`
* `dundas.bi.website.resources`
* `dundas.bi.scheduler.resources`
* `dundas.bi.authbridge.resources`
* `dundas.bi.authbridge.reverseproxy.resources`
* `dundas.bi.gatewayhub.resources`
* `dundas.bi.setup.orchestrator.resources`
* `dundas.bi.rabbitmq.resources`
* `dundas.bi.logreceiver.resources`

The initContainers resources will be the same as the pods corresponding limits and resource that it is initializing.

# Access under a path

If you want to access Dundas BI as a path under the site rather than at the root level, set `dundas.bi.virtualDirectoryPath` to the path, e.g. `/DBI/`.

# Pod Disruption Budget 

You can add a PodDisruptionBudget for each deployment included in the helm chart.  To do this define the following pdb section under the deployment sections like: dundas.bi.website, dundas.bi.scheduler, dundas.bi.authbridge, dundas.bi.gatewayhub, dundas.bi.rabbitmq.

```
pdb:
  enabled: true
  # Only one of the following can be defined for the PodDisruptionBudget.
  minAvailable: 1
  maxUnavailable: 1
```

An example for the Dundas BI website would be: 
```
dundas:
  bi:
    website:
	  pdb:
	    enabled: true
		minAvailable: 1
		
```
The Pod Disruption Budget can also be included for the express database deployment by setting the following:

```
dundas:
  bi:
    appDb:
        expresspdb:
          enabled: true
          minAvailable: 1
```

# Security Context and Deployment Settings

Dundas BI supports configuring security contexts, node selectors, affinity rules, tolerations, image pull policies, and restart policies at multiple levels. These settings follow a hierarchical precedence where the most specific configuration is applied first:

1. **Component-level settings** - Settings applied to a specific component; an example dundas.bi.website, or dundas.bi.authbridge (highest precedence)
2. **Global settings** - Settings applied at the global level
3. **Root/chart-level settings** - Default settings applied to all components (lowest precedence)

## Available Settings

The following settings can be configured at multiple levels:

| Setting | Description |
| ------- | ----------- |
| `securityContext` | Container-level security context settings |
| `podSecurityContext` | Pod-level security context settings |
| `nodeSelector` | Node selection constraints |
| `affinity` | Pod affinity/anti-affinity rules |
| `restartPolicy` | Pod restart policy (Always, OnFailure, Never). **Note:** Any Job will always have a restart policy of `OnFailure`, and this is not configurable. Additionally, the restart policy does not follow the level 4 of the priority order and instead defaults to `Always`. |

## Priority Order

When determining which configuration to use, the chart follows this order:

1. Look for component-specific setting (e.g., `dundas.bi.website.securityContext`, `dundas.bi.website.image.pullPolicy`)
2. If not found, look for global setting (e.g., `global.securityContext`, `global.image.pullPolicy`)
3. If not found, use chart-level setting (e.g., `securityContext`, `image.pullPolicy`)
4. If none are defined, use Kubernetes defaults

# Pod Annotations

Dundas BI supports configuring pod annotations at multiple levels similar to other deployment settings. Pod annotations are useful for integrating with service meshes, monitoring tools, or indicating deployment actions. 

## Annotation Levels

Annotations can be defined at three levels:

1. **Component-level** - Settings applied to a specific component (example: `dundas.bi.website.podAnnotations`)
2. **Global-level** - Settings applied at the global level (`global.podAnnotations`)
3. **Root/chart-level** - Settings applied to all components (`podAnnotations`)

When annotations are defined at multiple levels, they are merged together. If the same annotation key is defined at multiple levels, the component-level value (1) takes precedence over global-level (2), which takes precedence over root-level (3).
