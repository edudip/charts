# vaultwarden
A simple and lightweight Vaultwarden Helm Chart. [Vaultwarden](https://github.com/dani-garcia/vaultwarden) is former known as Bitwarden_RS, an alternative "alternative implementation of the Bitwarden server API written in Rust and compatible with [upstream Bitwarden clients](https://bitwarden.com/download/), perfect for self-hosted deployment where running the official resource-heavy service might not be ideal."

## TL;DR

```bash
$ helm repo add edudip https://charts.edudip.dev/
$ helm search repo edudip
$ helm install some-vaultwarden edudip/vaultwarden
```

## Prerequisites
- Kubernetes 1.21+
- Helm 3.9.4+

## Database

The value of `externalDatabase.type` switches between the different database configurations. If set to "-" (default) this chart will use 'SQLite3' database and needs a PersistentVolume or your data will be lost. 'MySQL' and 'PostgreSQL' can also be used as external database. Therfore the `externalDatabase.existingSecret.name` can be used, to provide a secret containing the full database URL string as the safest way. The secret should contain this database URL string with the key "url" or different values can be set with `externalDatabase.existingSecret.urlKey`. This string could also be provided in plain text in the values file as `externalDatabase.overrideUrl`, but keep in mind, this URL string contains the password and user to connect to the database.

Note: For PSQL a full PSQL Databas URL is expexted like this: "postgresql://[[user]:[password]@]host[:port][/database]"
Ref: https://github.com/dani-garcia/vaultwarden/wiki/Using-the-PostgreSQL-Backend

Note: For MariaDB a full MySQL Databas URL is expexted like this: "mysql://[[user]:[password]@]host[:port][/database]"
Ref: https://github.com/dani-garcia/vaultwarden/wiki/Using-the-MariaDB-%28MySQL%29-Backend

## Domain

Vaultwarden needs a valid domain under which this service is provided. The domain must be set with `domain`.

## Parameters

### "Common Parameters"

| Name                                            | Description                                                                      | Value   |
| ----------------------------------------------- | -------------------------------------------------------------------------------- | ------- |
| `image.repository`                              | Repository URI or name                                                           | `""`    |
| `image.pullPolicy`                              | Kubernetes pullPolicy: can 'IfNotPresent' or 'Always'                            | `""`    |
| `image.tag`                                     | Overrides the image tag whose default is the chart appVersion                    | `""`    |
| `image.digest`                                  | Sha256 value of the image. Note: if set `digest` will override the tag parameter | `""`    |
| `podSecurityContext.enabled`                    | Switches `securityContext` for the POD                                           | `false` |
| `containerSecurityContext.enabled`              | Switch `securityContext` for the container                                       | `false` |
| `resources`                                     | Defaults for the resources used by the application                               | `{}`    |
| `autoscaling.enabled`                           | Enable horizontal POD autoscaling                                                | `false` |
| `autoscaling.minReplicas`                       | Minimum number of replicas                                                       | `1`     |
| `autoscaling.maxReplicas`                       | Maximum number of replicas                                                       | `100`   |
| `autoscaling.targetCPUUtilizationPercentage`    | Target CPU utilization percentage                                                | `80`    |
| `autoscaling.targetMemoryUtilizationPercentage` | Target Memory utilization percentage                                             | `""`    |
| `replicaCount`                                  | Number of replicas to deploy                                                     | `1`     |
| `serviceAccount.create`                         | Specifies whether a service account should be created                            | `true`  |
| `serviceAccount.annotations`                    | Annotations to add to the service account                                        | `{}`    |
| `serviceAccount.name`                           | The name of the service account to use                                           | `""`    |
| `persistence.enabled`                           | Switches the persistance feature                                                 | `""`    |
| `persistence.existingClaim`                     | Name of an existing claim                                                        | `""`    |
| `persistence.accessModes`                       | Array of accessModes                                                             | `[]`    |
| `persistence.size`                              |                                                                                  | `""`    |
| `persistence.storageClassName`                  |                                                                                  | `""`    |
| `persistence.volumeName`                        | If `persistence.storageClassName` is set to "-"                                  | `""`    |

### "Traffic related parameters"

| Name                            | Description                                                           | Value   |
| ------------------------------- | --------------------------------------------------------------------- | ------- |
| `ingress.enabled`               | turn on/off ingress of the chart at all                               | `false` |
| `ingress.className`             | Name of the Ingress Class                                             | `""`    |
| `ingress.annotations`           | Annotations to add to the ingress object                              | `{}`    |
| `ingress.hosts`                 | Array of host objects                                                 | `[]`    |
| `ingress.tls`                   | Array of TLS configurations                                           | `[]`    |
| `ingress.servicePort`           | Port number of the service to reach HTTP endpoint defaults is port 80 | `""`    |
| `service.type`                  | Kubernetes service type                                               | `""`    |
| `service.ports`                 | List of service ports                                                 | `[]`    |
| `service.clusterIP`             | Service cluster IP                                                    | `""`    |
| `service.externalTrafficPolicy` | Service external traffic policy                                       | `[]`    |
| `service.sessionAffinity`       | Service session afffinity                                             | `""`    |
| `service.sessionAffinityConfig` | Additional settings for the sessionAffinity                           | `{}`    |

### "Vaultwarden Related Parameters"

| Name                                     | Description                                                                                      | Value   |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------ | ------- |
| `logLevel`                               | Verbosity level of the log output                                                                | `info`  |
| `domain`                                 | URL to access Vaultwarden instance                                                               | `""`    |
| `dataFolder`                             | Prefix to override the location where data is saved.                                             | `/data` |
| `externalDatabase.type`                  | Type of the databas (protocol)                                                                   | `""`    |
| `externalDatabase.username`              | Username of the database user                                                                    | `""`    |
| `externalDatabase.password`              | Password of the database                                                                         | `""`    |
| `externalDatabase.host`                  | URN of the database server (can be IP or domain)                                                 | `""`    |
| `externalDatabase.port`                  | Port of the database server                                                                      | `""`    |
| `externalDatabase.database`              | Name of the database                                                                             | `""`    |
| `externalDatabase.overrideUrl`           | If `externalDatabase.type` is not "-", overrides whole database URL and ignores other parameters | `""`    |
| `externalDatabase.existingSecret.name`   | Name of the secret containing complete database URL                                              | `""`    |
| `externalDatabase.existingSecret.urlKey` | Key of the databas URL (if let empfty "url" will be used)                                        | `""`    |
| `smtp.urn`                               | URN of the SMTP server (can be IP or domain)                                                     | `""`    |
| `smtp.security`                          | Security option of the SMTP server (can be "starttls", "force_tls", "off")                       | `""`    |
| `smtp.port`                              | Port of the SMTP server                                                                          | `""`    |
| `smtp.username`                          | Username of SMTP server (leave empty if not needed)                                              | `""`    |
| `smtp.password`                          | Password of SMTP server (leave empty if not needed)                                              | `""`    |
| `smtp.from`                              | Email address of the sender (should not be empty or invalid)                                     | `""`    |
| `smtp.existingSecret.name`               |                                                                                                  | `""`    |
| `smtp.existingSecret.usernameKey`        | Key of the SMTP username (if let empty "username" will be used)                                  | `""`    |
| `smtp.existingSecret.passwordKey`        | Key of the SMTP password (if let empty "password" will be used)                                  | `""`    |
| `signups.allowed`                        | If true anyone can access the instance of Vaultwarden                                            | `""`    |
| `signups.domainWhitelist`                | Restrics registration to this email addresses from certain domains                               | `""`    |
| `invitationsAllowed`                     | If set to false no invitation is possible                                                        | `""`    |
| `websocketEnabled`                       | WebSocket notifications are used to inform the browser and desktop Bitwarden clients             | `""`    |
| `webVaultEnabled`                        | This parameter disables this static file hosting completely if set to 'false'                    | `""`    |
| `showPasswordHint`                       | Usually, password hints are sent by email.                                                       | `""`    |
| `adminToken`                             | The `/admin` page will be enabled if set any token                                               | `""`    |
| `adminTokenSecret.name`                  | Name of the secret containing the `ADMIN_TOKEN`                                                  | `""`    |
| `adminTokenSecret.tokenKey`              | Key of the token (if let empty "token" will be used)                                             | `""`    |
| `disableAdminToken`                      | If set to true built in `ADMIN_TOKEN` will be disabled                                           | `""`    |
| `yubikey.clientID`                       | Your Yubikey client ID                                                                           | `""`    |
| `yubikey.secretKey`                      | Your Yubikey secret key                                                                          | `""`    |
| `yubikey.server`                         | If not set points to YubiCloud servers                                                           | `""`    |
| `rocket.tls`                             | To enable HTTPS in Vaultwarden itself, set the ROCKET_TLS environment variable                   | `""`    |
| `rocket.limits`                          | By default the API calls are limited to 10MB                                                     | `""`    |
| `rocket.workers`                         | Set the number of rocket workers by hand                                                         | `""`    |
| `writeAheadLoggingEnabled`               | Switches Write-Ahead Logging of SQLite database                                                  | `""`    |


