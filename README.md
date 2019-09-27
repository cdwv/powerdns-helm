# Helm Chart for PowerDNS

* Installs a PowerDNS authoritative nameserver

## TL;DR;

```console
$ git clone https://github.com/cdwv/powerdns-helm
$ cd powerdns-helm
$ helm install .
```

> Note: This will leave you with a PowerDNS server deployed to your k8s cluster. You will also need to configure some means of accesibility from the outside world for the DNS server to be accesible. You can do it e.g. by modifying your nginx-ingress-controller.

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm install --name my-release .
```

## Uninstalling the Chart

To uninstall/delete the my-release deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.


## Configuration

### PowerDNS

| Parameter                  | Description                         | Default                                                 |
|----------------------------|-------------------------------------|---------------------------------------------------------|
| `pdns.api.enabled`         | Should the PowerDNS API be enabled | `yes`
| `pdns.api.key`             | PowerDNS API key | `PowerDNSAPI`
| `pdns.webserver.allowFrom` | PowerDNS webserver allowed IP whitelist |  `0.0.0.0/0`
| `pdns.dnsupdate.enabled`   | Should DNS UPDATE support be enabled | `no` |
| `replicaCount`                 | Number of pdns nodes | `1` |
| `image.repository`         | Image repository | `psitrax/powerdns` |
| `image.tag`                | Image tag. | `4.1.2`|
| `image.pullPolicy`         | Image pull policy | `IfNotPresent` |
| `service.type`             | Kubernetes service type | `ClusterIP` |
| `service.clusterIP`        | Kubernetes service ClusterIP | `nil` |
| `ingress.enabled`          | Enables Ingress | `false` |
| `ingress.annotations`      | Ingress annotations | `{}` |
| `ingress.path`           | Custom path                       | `/`
| `ingress.hosts`            | Ingress accepted hostnames | `chart-example.local` |
| `ingress.tls`              | Ingress TLS configuration | `[]` |
| `resources`                | CPU/Memory resource limits/requests | `{}` |
| `nodeSelector`             | Node labels for pod assignment | `{}` |
| `tolerations`              | Toleration labels for pod assignment | `[]` |
| `affinity`                 | Affinity settings for pod assignment | `{}` |
| `mariadb.enabled`                    | Deploy MariaDB container(s)                | `true`     |
| `mariadb.mariadbRootPassword`        | MariaDB admin password                     | `nil`      |
| `mariadb.mariadbDatabase`            | Database name to create                    | `powerdns` |
| `mariadb.mariadbUser`                | Database user to create                    | `powerdns` |
| `mariadb.mariadbPassword`            | Password for the database                  | `powerdns` |
| `mariadb.persistence.enabled`                | Enable persistence using PVC               | `false`   |
| `mariadb.persistence.existingClaim`          | Enable persistence using an existing PVC   | `nil`                                                      |
| `mariadb.persistence.storageClass`           | PVC Storage Class                          | `nil` (uses alpha storage class annotation)                |
| `mariadb.persistence.accessMode`             | PVC Access Mode                            | `ReadWriteOnce`                                            |
| `mariadb.persistence.size`                   | PVC Storage Request                        | `2Gi`   |


# Modifications to nginx-ingress

General steps are outlined below. You can consult the files in the nginx-ingress folder for more information.

1. Patch the nginx-ingress deployment to add UDP services (--udp-services-configmap=nginx-ingress/udp-ports)
2. Add udp-ports configmap to nginx-ingress namespace
2. Add a UDP loadbalancer service

# TODO

* [ ] - Make sure all kubernetes/charts [technical requirements](https://github.com/kubernetes/charts/blob/master/CONTRIBUTING.md#technical-requirements) are met
* [ ] - Make sure all kubernetes/charts [documentation requirements](https://github.com/kubernetes/charts/blob/master/CONTRIBUTING.md#documentation-requirements) are met
* [ ] - Add more pdns configuration options
* [ ] - Support secrets
