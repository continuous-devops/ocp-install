# os-install
Repo to grab and run the openshift-install for 4.0 and greater clusters using a release or ci version

## Settings and defaults

```
    base_dir: "~/.os-install"
    cluster_name: "ttcd-ari"
    os_install_name: "openshift-install"
    install_dir: "{{ base_dir }}/install"
    cluster_dir:  "{{ base_dir }}/{{ cluster_name }}"
    aws_region: "us-east-1"
    pull_secret_file: "~/.aws/pull-secret.json"
    ssh_key_file: "~/.ssh/id_rsa.pub"
    get_release: true
    create_cluster: true
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