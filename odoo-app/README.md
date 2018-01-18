# Odoo Chart for ICP

[Odoo](https://www.odoo.com/) is a suite of web based open source business apps. The main Odoo Apps include an Open Source CRM, Website Builder, eCommerce, Project Management, Billing & Accounting, Point of Sale, Human Resources, Marketing, Manufacturing, Purchase Management, ...

Odoo Apps can be used as stand-alone applications, but they also integrate seamlessly so you get a full-featured Open Source ERP when you install several Apps.

This version is build to run on ppc64le compatible Hardware (IBM PowerSystem with Power8 minimum or OpenPower comatible system)

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release odoo-app
```

> **Tip**: List all releases using `helm list`

## Verifying the Chart
See NOTES.txt associated with this chart for verification instructions

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.  If a delete can result in orphaned components include instructions additional commands required for clean-up.  

For example :

When deleting a release with stateful sets the associated persistent volume will need to be deleted.  
Do the following after deleting the chart release to clean up orphaned Persistent Volumes.

```console
$ kubectl delete pvc -l release=my-release
``` 

## Configuration
The following tables lists the configurable parameters of the odoo chart and their default values.

| Parameter                            | Description                                     | Default                                                    |
| ----------------------------------   | ---------------------------------------------   | ---------------------------------------------------------- |
| `arch.amd64`                  | `Amd64 worker node scheduler preference in a hybrid cluster` | `2 - No preference` - worker node is chosen by scheduler       |
| `arch.ppc64le`                | `Ppc64le worker node scheduler preference in a hybrid cluster` | `2 - No preference` - worker node is chosen by scheduler       |
| `arch.s390x`                  | `S390x worker node scheduler preference in a hybrid cluster` | `2 - No preference` - worker node is chosen by scheduler       |
| `image`                              | `odoo` image repository                   | `schabrolles/odoo_ppc64le`                                       |
| `imageTag`                           | `odoo` image tag                          | `icp`                                                    |
| `imagePullPolicy`                    | Image pull policy                               | `Always` if `imageTag` is `latest`, else `IfNotPresent`    |
| `resources`                          | CPU/Memory resource requests/limits             | Memory: `256Mi`, CPU: `100m`                               |
| `service.port`                       | TCP port                                        | `5432`                                                     |
| `service.type`                       | k8s service type exposing ports, e.g. `NodePort`| `NodePort`                                                 |
| `odoo.admin_password`                | odoo super-user admin password                  | `admin`                                                    |
| `database.dbhost`                    | postgresql database (service-name) on which odoo has to connect to | need to be specified |
| `database.pgpassword`                | postgresql super-admin password (usually postgres user) | need to be specified |
| `database.dbuser`                    | postgresql user used by odoo to connect to postgres | `odoo` |
| `database.dbpassword`                | password link to the odoo postgres user | `odoo` |


Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

> **Tip**: You can use the default [values.yaml](values.yaml)

## Architecture

- Three major architectures are now available for odoo n on IBM Cloud Private worker nodes:
  - AMD64 / x86_64
  - s390x
  - ppc64le

An ‘arch’ field in values.yaml is required to specify supported architectures to be used during scheduling and includes ability to give preference to certain architecture(s) over another.

Specify architecture (amd64, ppc64le, s390x) and weight to be  used for scheduling as follows :
   0 - Do not use
   1 - Least preferred
   2 - No preference
   3 - Most preferred

