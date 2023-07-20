# Tanzu GitOps for .NET DevX

For detailed documentation, refer to [Install Tanzu Application Platform through Gitops with Secrets OPerationS (SOPS)
](https://docs-staging.vmware.com/en/draft/VMware-Tanzu-Application-Platform/1.6/tap/install-gitops-sops.html)

## Prereqs

* [Tanzu CLI](https://docs-staging.vmware.com/en/draft/VMware-Tanzu-Application-Platform/1.6/tap/install-tanzu-cli.html)
* [SOPS CLI](https://github.com/getsops/sops)
* [age CLI](https://github.com/FiloSottile/age)
* GKE
  * [gcloud CLI](https://cloud.google.com/sdk/gcloud)

## Cluster Preparation

### GKE Cluster

*Setup*

```sh
bin/gke-cluster-setup -y CLUSTER
```

*Authenticate*

If you've created the cluster as outlined above, you can skip following step.

```sh
bin/gke-cluster-authenticate CLUSTER
```

*Teardown*

Should you ever need to teardown the cluster, run:

```sh
bin/gke-cluster-teardown CLUSTER
```

### Tanzu Cluster Essentials

*Deploy*

```sh
bin/tce-deploy -y CLUSTER
```

## TAP

### Setup

Setup repo for a new GitOps TAP configuration.

```sh
bin/ops-setup -g CLUSTER
```

The switch `-g` commits the setup, with the exception of the TAP configuration files, to the repo with the message `CLUSTER: setup`.

**! IMPORTANT !**

An Age key for the TAP cluster will be created at `keys/CLUSTER.age`.
Copy/backup the key to a safe place.

### Configure

*Edit*

* `clusters/CLUSTER/cluster-config/values/tap-install-values.yaml`
* `clusters/CLUSTER/cluster-config/values/tap-sensitive-values.yaml`
* `clusters/CLUSTER/cluster-config/values/tanzu-sync.yaml`
* `clusters/CLUSTER/cluster-config/sensitive-values/tanzu-sync-values.yaml`

*Encrypt Sensitive Values*

```sh
bin/ops-secrete CLUSTER
```

*Push Configuration*

```sh
git add .
git commit -m"CLUSTER: configure"
git push
```

### Deploy

```sh
bin/ops-deploy -y CLUSTER
```
