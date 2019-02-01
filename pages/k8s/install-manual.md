---
wrapper_template: "base_docs.html"
markdown_includes:
  nav: "shared/_side-navigation.md"
context:
  title: "Installing CDK manually"
  description: How to install and customise the Charmed Distribution of Kubernetes using Juju bundles.
keywords: lxd, conjure-up, requirements,developer
tags: [install]
sidebar: k8smain-sidebar
permalink: install-manual.html
layout: [base, ubuntu-com]
toc: False
---

The recommended way to install the
**Charmed Distribution of Kubernetes <sup>&reg;</sup>** is using the
**conjure-up** install tool as described in the
['Quick start' documentation][quickstart]. However, in some cases it may be useful to
customise the install:

  - Setting specific options for the master/worker nodes
  - Adding additional components (e.g. monitoring)
  - Configuring storage or networking  
  - Copying an existing configuration
  - Testing a pre-release version
  - ...and many more

In many cases, using the bundle install method outlined here will be faster and more
repeatable than customising a deployment post-install.

This documentation first presents the method for installing from the official release bundles, then explains how the bundle can be customised.


## Install CDK from the official bundle

The following sections outline a standard installation of CDK using the stable release Juju bundles. The standard bundle includes all the components of Kubernetes, but you will have to also follow the [additional configuration][cloud-specific] steps at the end for Kubernetes to function properly - additional components are required to enable Kubernetes to interact with the cloud it is deployed on.

###  Install Juju

If Juju has not already been installed on your system, you need to install it first. For Ubuntu 16.04 and later, and other operating systems which support [snaps][snaps], run the command:

```bash
sudo snap install juju --classic
```

For other install options, please see the [Juju documentation][juju-docs].

### Create a controller

Juju requires a *controller*  instance to manage models and deployed applications.
You can make use of a hosted controller ([JAAS][jaas]) or create one in a cloud of
your choice. For public clouds, you will need to have added a credential first:

```bash
juju add-credential
```

...will step through adding your credential for a specific cloud.


The Juju documentation has more information on [adding credentials][credentials] and [configuring a controller][controller-config]

<div class="p-notification--caution">
  <p markdown="1" class="p-notification__response">
    <span class="p-notification__status">Note:</span>
To manually install CDK locally using LXD, it is necessary to use a specific container profile before deploying.  See the <a href="/kubernetes/clouds-lxd">notes on LXD</a> before deployment.
  </p>
</div>



### Create a model

The controller automatically creates a model named 'default'. It is useful to have a different model for each deployment of CDK, and for the models to have useful names. You can create a new model with the `add-model` command like this:

```bash
juju add-model cdk-test
```

...susbstituting the desired name.

### Deploy the CDK bundle

The Juju Charm Store hosts the CDK bundles as well as individual charms.  To deploy
the latest, stable bundle, run the command:

```bash
juju deploy cs:~containers/canonical-kubernetes
```

It is also possible to deploy a specific version of the bundle by including the
revision number. For example, to deploy the CDK bundle for the Kubernetes 1.12
release, you could run:

```bash
juju deploy cs:~containers/canonical-kubernetes-296
```

The revision numbers for bundles are generated automatically when the bundle is
updated, including for testing and beta versions, so it isn't always the case
that a higher revision number is 'better'. The revision numbers for the release
versions of the CDK bundle are shown in the table below:

| Kubernetes version | CDK bundle |
| --- | --- |
| 1.13.x         | [canonical-kubernetes-363](https://api.jujucharms.com/charmstore/v5/~containers/bundle/canonical-kubernetes-363/archive/bundle.yaml?channel=stable) |
| 1.12.x         | [canonical-kubernetes-296](https://api.jujucharms.com/charmstore/v5/~containers/bundle/canonical-kubernetes-296/archive/bundle.yaml?channel=stable) |
| 1.11.x         | [canonical-kubernetes-217](https://api.jujucharms.com/charmstore/v5/~containers/bundle/canonical-kubernetes-217/archive/bundle.yaml?channel=stable) |
| 1.10.5+         | [canonical-kubernetes-211](https://api.jujucharms.com/charmstore/v5/~containers/bundle/canonical-kubernetes-211/archive/bundle.yaml?channel=stable)  |
| 1.10.x         | [canonical-kubernetes-188](https://api.jujucharms.com/charmstore/v5/~containers/bundle/canonical-kubernetes-188/archive/bundle.yaml?channel=stable) |
| 1.9.3+         | [canonical-kubernetes-179](https://api.jujucharms.com/charmstore/v5/~containers/bundle/canonical-kubernetes-179/archive/bundle.yaml?channel=stable) |
| 1.9.x        | [canonical-kubernetes-150](https://api.jujucharms.com/charmstore/v5/~containers/bundle/canonical-kubernetes-150/archive/bundle.yaml?channel=stable) |
| 1.8.x | [canonical-kubernetes-132](https://api.jujucharms.com/charmstore/v5/~containers/bundle/canonical-kubernetes-132/archive/bundle.yaml?channel=stable) |
| 1.7.x | [canonical-kubernetes-101](https://api.jujucharms.com/charmstore/v5/~containers/bundle/canonical-kubernetes-101/archive/bundle.yaml?channel=stable) |
| 1.6.x | [canonical-kubernetes-38](https://api.jujucharms.com/charmstore/v5/~containers/bundle/canonical-kubernetes-38/archive/bundle.yaml?channel=stable) |

<div class="p-notification--caution">
  <p markdown="1" class="p-notification__response">
    <span class="p-notification__status">Note:</span>
Only the latest three versions of CDK are supported at any time.
  </p>
</div>

### Additional configuration

To allow Kubernetes to access resources and functionality of the underlying
cloud it is deployed on, additional integrator charms are available. When
installing with **conjure-up**, these charms are automatically added to the
deployment and configured accordingly. This table explains which charms are
used:

| AWS        | aws-integrator       | juju deploy cs:~containers/aws-integrator       | https://jujucharms.com/u/containers/aws-integrator/       |
|------------|----------------------|-------------------------------------------------|-----------------------------------------------------------|
| Azure      | azure-integrator     | juju deploy cs:~containers/azure-integrator     | https://jujucharms.com/u/containers/azure-integrator/     |
| Google     | gcp-integrator       | juju deploy cs:~containers/gcp-integrator       | https://jujucharms.com/u/containers/gcp-integrator/       |
| Local      | N/A                  | N/A                                             |                                                           |
| OpenStack  | openstack-integrator | juju deploy cs:~containers/openstack-integrator | https://jujucharms.com/u/containers/openstack-integrator/ |
| Rackspace  | openstack-integrator | juju deploy cs:~containers/openstack-integrator | https://jujucharms.com/u/containers/openstack-integrator/ |
| vSphere    | vsphere-integrator   | juju deploy cs:~containers/vsphere-integrator   | https://jujucharms.com/u/containers/vsphere-integrator/   |

The charm should be deployed and relationships established with both the `kubernetes-worker` and `kubernetes-master` charms. For example, in the case of AWS:

```bash
juju deploy cs:~containers/aws-integrator
juju trust aws-integrator
juju add-relation aws-integrator kubernetes-master
juju add-relation aws-integrator kubernetes-worker
```

The `juju trust` command allows the aws-integrator to make use of the
credentials stored by Juju.

Whilst it isn't particularly onerous to do this step manually once or twice, for repeated use, you may wish to investigate the options on customising your install below.

## Customising the bundle install

A number of the scenarios outlined at the start of this document involved
customising  the **CDK** install. There are two main ways to do this, using
overlays or editing the bundle file itself.

Using an overlay means you can easily apply your customisation to different
versions of the **CDK** bundle, with the possible downside that changes in the
structure of  new versions of **CDK** may render your overlay obsolete or
non-functional (depending on what exactly your overlay does).

Saving a copy of the **CDK** bundle file and editing that means that your
customisation will always work, but of course, requires that you create a new
file for each version of **CDK**.

Both methods are described below.

### Using overlays

A _bundle overlay_ is a fragment of valid YAML which is dynamically merged on
top of a bundle before deployment, rather like a patch file. The fragment can
contain any additional or alternative YAML which is intelligible to Juju. For
example, to replicate the steps used above to deploy and connect the
aws-integrator charm, the following fragment could be used:

```yaml
applications:
  aws-integrator:
    charm: cs:~containers/aws-integrator
    num_units: 1
relations:
  - ['aws-integrator', 'kubernetes-master']
  - ['aws-integrator', 'kubernetes-worker']
  ```

You can also [download the fragment here][asset-aws-overlay]

Juju's bundle format, and valid YAML are discussed more fully in the
[Juju documentation][juju-bundle]. In this case it merely adds a new application,
specifying the charm to use, and further specifies the relationships to add.

To use this overlay with the **CDK** bundle, it is specified during deploy like this:

```bash
juju deploy cs:~containers/canonical-kubernetes  --overlay ~/path/aws-overlay.yaml
```

Substitute in the local path and filename to point to your YAML fragment.

#### Adding or changing constraints

After adding additional components, the most common use of overlays is to change
constraints (the resources requested for the application). Although these are specified
already in the **CDK** bundle, they can be overridden by an overlay. It isn't necessary
to replicate the entirety of an entry, just the parts you wish to change. For example:

```yaml
kubernetes-worker:
  constraints: cores=4 mem=8G root-disk=100G
  num_units: 6
```
Changes the machine constraints for Kubernetes workers to add more root disk space,
and also deploys six units instead of the three specified in the original bundle.

More information on the constraints you can use is available in the
[Juju documentation][juju-constraints].


### Editing a bundle
#### Fetching the latest CDK bundle
#### Retrieving a bundle from a running model

<!-- IMAGES -->
<!-- LINKS -->

[juju-docs]: https://docs.jujucharms.com/reference-install
[controller-config]: https://docs.jujucharms.com/controllers-config
[credentials]: https://docs.jujucharms.com/
[quickstart]: /kubernetes/quickstart
[clouds-lxd]: /kubernetes/clouds-lxd
[juju-bundle]: https://docs.jujucharms.com/stable/en/charms-bundles
[juju-constraints]: https://docs.jujucharms.com/stable/en/reference-constraints
[asset-aws-overlay]: https://raw.githubusercontent.com/juju-solutions/kubernetes-docs/master/assets/aws-overlay.yaml
