# os-install
Repo to grab and run the openshift-install for 4.0 and greater clusters using a release or ci version

## Settings and defaults

```
    base_dir: "~/.os-install"
    ocp_install_dir: "{{ base_dir }}/ocp"
    ocp_ci_release: "4.3.0-0.ci"
    ocp_release: "latest"
    ocp_rel_url: "https://mirror.openshift.com/pub/openshift-v4/clients/ocp"
    ocp_ci_api_rel_url: "https://openshift-release.svc.ci.openshift.org/api/v1/releasestream"
    ocp_ci_rel_url: "https://openshift-release-artifacts.svc.ci.openshift.org"
    cluster_name: "ocp-cluster-4"
    cluster_dir:  "{{ base_dir }}/{{ cluster_name }}"
    aws_region: "us-east-1"
    pull_secret_file: "~/.aws/pull-secret.json"
    ssh_key_file: "~/.ssh/id_rsa.pub"
    ssh_key: "{{ lookup('file', ssh_key_file) }}"
    get_release: true
    create_cluster: true
```

### ocp_release
Stable released version of OCP installer and client- ex. latest
Other examples - latest-4.1, stable-4.2, etc.
```
default: "latest"

```

### ocp_ci_release
Stable CI release version of OCP installer and client - ex. 4.3.0-0.ci
Other examples - 4.3.0-0.nightly, 4.2.0-0.ci
```
default: "4.4.0-0.ci"

```

### aws_region
Region to setup your cluster
```
default: "us-east-1"

```

### pull_secret_file
Location of your pull secret file
```
default: "~/.aws/pull-secret.json"

```

### ssh_key_file
Location of your public ssh key file
```
default: "~/.ssh/id_rsa.pub"

```

### get_release
This means the latest release of the openshift-installer will get downloaded otherwise it pulls from
the latest version that passed ci.

```
default: true

```

### create_cluster
```
default: true

```
This will create a cluster otherwise will just destroy a previous one with the same name

## Examples

```
ansible-playbook -vv -i "localhost," -c local ans_run_installer.yml

```