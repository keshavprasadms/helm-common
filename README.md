### Common library chart
A library helm chart for common templates

```
dependencies:
  - name: common
    version: 1.x.x
    repository:
```
Run the below command to update dependencies
```
$ helm dependency update
```

## Introduction

This chart provides a common template helpers which can be used to develop new charts using [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+

## Parameters

The following table lists the helpers available in the library which are scoped in different sections.

### Images

| Helper identifier           | Description                                          | Expected Input                                                                                          |
|-----------------------------|------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| `common.images.image`       | Return the proper and full image name                | `dict "imageRoot" .Values.path.to.the.image "global" $`, see [ImageRoot](#imageroot) for the structure. |
| `common.images.renderPullSecrets` | Return the proper Docker Image Registry Secret Names (evaluates values as templates) | `dict "images" (list .Values.path.to.the.image1, .Values.path.to.the.image2) "context" $` |

### Labels

| Helper identifier           | Description                                                                 | Expected Input    |
|-----------------------------|-----------------------------------------------------------------------------|-------------------|
| `common.labels.standard`    | Return Kubernetes standard labels                                           | `.` Chart context |
| `common.labels.matchLabels` | Labels to use on `deploy.spec.selector.matchLabels` and `svc.spec.selector` | `.` Chart context |

### Names

| Helper identifier                 | Description                                                           | Expected Input    |
|-----------------------------------|-----------------------------------------------------------------------|-------------------|
| `common.names.name`               | Expand the name of the chart or use `.Values.nameOverride`            | `.` Chart context |
| `common.names.fullname`           | Create a default fully qualified app name.                            | `.` Chart context |
| `common.names.namespace`          | Allow the release namespace to be overridden                          | `.` Chart context |
| `common.names.fullname.namespace` | Create a fully qualified app name adding the installation's namespace | `.` Chart context |
| `common.names.chart`              | Chart name plus version                                               | `.` Chart context |

### Storage

| Helper identifier             | Description                           | Expected Input                                                                                                      |
|-------------------------------|---------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| `common.storage.class` | Return  the proper Storage Class | `dict "persistence" .Values.path.to.the.persistence "global" $`, see [Persistence](#persistence) for the structure. |

### Capabilities

| Helper identifier                              | Description                                                                                    | Expected Input    |
|------------------------------------------------|------------------------------------------------------------------------------------------------|-------------------|
| `common.capabilities.kubeVersion`              | Return the target Kubernetes version (using client default if .Values.kubeVersion is not set). | `.` Chart context |
| `common.capabilities.cronjob.apiVersion`       | Return the appropriate apiVersion for cronjob.                                                 | `.` Chart context |
| `common.capabilities.deployment.apiVersion`    | Return the appropriate apiVersion for deployment.                                              | `.` Chart context |
| `common.capabilities.statefulset.apiVersion`   | Return the appropriate apiVersion for statefulset.                                             | `.` Chart context |
| `common.capabilities.ingress.apiVersion`       | Return the appropriate apiVersion for ingress.                                                 | `.` Chart context |
| `common.capabilities.rbac.apiVersion`          | Return the appropriate apiVersion for RBAC resources.                                          | `.` Chart context |
| `common.capabilities.crd.apiVersion`           | Return the appropriate apiVersion for CRDs.                                                    | `.` Chart context |
| `common.capabilities.policy.apiVersion`        | Return the appropriate apiVersion for podsecuritypolicy.                                       | `.` Chart context |
| `common.capabilities.networkPolicy.apiVersion` | Return the appropriate apiVersion for networkpolicy.                                           | `.` Chart context |
| `common.capabilities.apiService.apiVersion`    | Return the appropriate apiVersion for APIService.                                              | `.` Chart context |
| `common.capabilities.hpa.apiVersion`           | Return the appropriate apiVersion for Horizontal Pod Autoscaler                                | `.` Chart context |
| `common.capabilities.supportsHelmVersion`      | Returns true if the used Helm version is 3.3+                                                  | `.` Chart context |

### Affinities

| Helper identifier             | Description                                          | Expected Input                                 |
|-------------------------------|------------------------------------------------------|------------------------------------------------|
| `common.affinities.nodes.soft` | Return a soft nodeAffinity definition                | `dict "key" "FOO" "values" (list "BAR" "BAZ")` |
| `common.affinities.nodes.hard` | Return a hard nodeAffinity definition                | `dict "key" "FOO" "values" (list "BAR" "BAZ")` |
| `common.affinities.pods.soft`  | Return a soft podAffinity/podAntiAffinity definition | `dict "component" "FOO" "context" $`           |
| `common.affinities.pods.hard`  | Return a hard podAffinity/podAntiAffinity definition | `dict "component" "FOO" "context" $`           |

### TplValues

| Helper identifier         | Description                            | Expected Input                                                                                                                                           |
|---------------------------|----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `common.tplvalues.render` | Renders a value that contains template | `dict "value" .Values.path.to.the.Value "context" $`, value is the value should rendered as template, context frequently is the chart context `$` or `.` |

### Secrets

| Helper identifier         | Description                                                  | Expected Input                                                                                                                                                                                                                  |
|---------------------------|--------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `common.secrets.name`     | Generate the name of the secret.                             | `dict "existingSecret" .Values.path.to.the.existingSecret "defaultNameSuffix" "mySuffix" "context" $` see [ExistingSecret](#existingsecret) for the structure.                                                                  |
| `common.secrets.key`      | Generate secret key.                                         | `dict "existingSecret" .Values.path.to.the.existingSecret "key" "keyName"` see [ExistingSecret](#existingsecret) for the structure.                                                                                             |
| `common.passwords.manage` | Generate secret password or retrieve one if already created. | `dict "secret" "secret-name" "key" "keyName" "providedValues" (list "path.to.password1" "path.to.password2") "length" 10 "strong" false "chartName" "chartName" "context" $`, length, strong and chartNAme fields are optional. |
| `common.secrets.exists`   | Returns whether a previous generated secret already exists.  | `dict "secret" "secret-name" "context" $`

### Utils

| Helper identifier              | Description                                                                              | Expected Input                                                         |
|--------------------------------|------------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| `common.utils.fieldToEnvVar`   | Build environment variable name given a field.                                           | `dict "field" "my-password"`                                           |
| `common.utils.secret.getvalue` | Print instructions to get a secret value.                                                | `dict "secret" "secret-name" "field" "secret-value-field" "context" $` |
| `common.utils.getValueFromKey` | Gets a value from `.Values` object given its key path                                    | `dict "key" "path.to.key" "context" $`                                 |
| `common.utils.getKeyFromList`  | Returns first `.Values` key with a defined value or first of the list if all non-defined | `dict "keys" (list "path.to.key1" "path.to.key2") "context" $`         |

### Validations

| Helper identifier                                | Description                                                                                                                   | Expected Input                                                                                                                                                                                                                                                           |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `common.validations.values.single.empty`         | Validate a value must not be empty.                                                                                           | `dict "valueKey" "path.to.value" "secret" "secret.name" "field" "my-password" "subchart" "subchart" "context" $` secret, field and subchart are optional. In case they are given, the helper will generate a how to get instruction. See [ValidateValue](#validatevalue) |
| `common.validations.values.multiple.empty`       | Validate a multiple values must not be empty. It returns a shared error for all the values.                                   | `dict "required" (list $validateValueConf00 $validateValueConf01) "context" $`. See [ValidateValue](#validatevalue)                                                                                                                                                      |
| `common.validations.values.mariadb.passwords`    | This helper will ensure required password for MariaDB are not empty. It returns a shared error for all the values.            | `dict "secret" "mariadb-secret" "subchart" "true" "context" $` subchart field is optional and could be true or false it depends on where you will use mariadb chart and the helper.                                                                                      |
| `common.validations.values.mysql.passwords`      | This helper will ensure required password for MySQL are not empty. It returns a shared error for all the values.              | `dict "secret" "mysql-secret" "subchart" "true" "context" $` subchart field is optional and could be true or false it depends on where you will use mysql chart and the helper.                                                                                      |
| `common.validations.values.postgresql.passwords` | This helper will ensure required password for PostgreSQL are not empty. It returns a shared error for all the values.         | `dict "secret" "postgresql-secret" "subchart" "true" "context" $` subchart field is optional and could be true or false it depends on where you will use postgresql chart and the helper.                                                                                |
| `common.validations.values.redis.passwords`      | This helper will ensure required password for Redis&reg; are not empty. It returns a shared error for all the values. | `dict "secret" "redis-secret" "subchart" "true" "context" $` subchart field is optional and could be true or false it depends on where you will use redis chart and the helper.                                                                                          |
| `common.validations.values.cassandra.passwords`  | This helper will ensure required password for Cassandra are not empty. It returns a shared error for all the values.          | `dict "secret" "cassandra-secret" "subchart" "true" "context" $` subchart field is optional and could be true or false it depends on where you will use cassandra chart and the helper.                                                                                  |
| `common.validations.values.mongodb.passwords`    | This helper will ensure required password for MongoDB&reg; are not empty. It returns a shared error for all the values.            | `dict "secret" "mongodb-secret" "subchart" "true" "context" $` subchart field is optional and could be true or false it depends on where you will use mongodb chart and the helper.                                                                                      |

### Errors

| Helper identifier                       | Description                                                                                                                                                            | Expected Input                                                                      |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| `common.errors.upgrade.passwords.empty` | It will ensure required passwords are given when we are upgrading a chart. If `validationErrors` is not empty it will throw an error and will stop the upgrade action. | `dict "validationErrors" (list $validationError00 $validationError01)  "context" $` |

## Special input schemas

### ImageRoot

```yaml
registry:
  type: string
  description: Docker registry where the image is located
  example: docker.io

repository:
  type: string
  description: Repository and image name
  example: bitnami/nginx

tag:
  type: string
  description: image tag
  example: 1.16.1-debian-10-r63

pullPolicy:
  type: string
  description: Specify a imagePullPolicy. Defaults to 'Always' if image tag is 'latest', else set to 'IfNotPresent'

pullSecrets:
  type: array
  items:
    type: string
  description: Optionally specify an array of imagePullSecrets (evaluated as templates).

debug:
  type: boolean
  description: Set to true if you would like to see extra information on logs
  example: false

## An instance would be:
# registry: docker.io
# repository: bitnami/nginx
# tag: 1.16.1-debian-10-r63
# pullPolicy: IfNotPresent
# debug: false
```

### Persistence

```yaml
enabled:
  type: boolean
  description: Whether enable persistence.
  example: true

storageClass:
  type: string
  description: Ghost data Persistent Volume Storage Class, If set to "-", storageClassName: "" which disables dynamic provisioning.
  example: "-"

accessMode:
  type: string
  description: Access mode for the Persistent Volume Storage.
  example: ReadWriteOnce

size:
  type: string
  description: Size the Persistent Volume Storage.
  example: 8Gi

path:
  type: string
  description: Path to be persisted.
  example: /bitnami

## An instance would be:
# enabled: true
# storageClass: "-"
# accessMode: ReadWriteOnce
# size: 8Gi
# path: /bitnami
```