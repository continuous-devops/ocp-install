# ocp-install
- Install prereqs of go, kubectl, jq, and yq
- Download release or version that passed ci of OCP which includes the openshift-installer and oc
- Install a cluster on GCP or AWS
- Install OpenShift Pipelines/Tektoncd Pipelines

## Settings and defaults

- playbooks/group_vars/all/vars.yml

```
    ## Default overall ocp cluster variable settings
    base_dir: "~/.ocp-install"
    bin_dir: "{{ base_dir }}/bin"
    install_prereqs: true
    install_ocp_bins: true
    install_cluster: true
    install_tekton: true
    
    ## Binary prereqs
    kubectl_version: "1.18.2"
    go_version: "1.14.2"
    jq_version: "1.6"
    yq_version: "2.4.1"
    
    ## Get OCP installer and client
    ocp_install_dir: "{{ base_dir }}/ocp"
    ocp_ci_release: "4.4.0-0.ci"
    ocp_release: "latest"
    ocp_rel_url: "https://mirror.openshift.com/pub/openshift-v4/clients/ocp"
    ocp_ci_api_rel_url: "https://openshift-release.svc.ci.openshift.org/api/v1/releasestream"
    ocp_ci_rel_url: "https://openshift-release-artifacts.svc.ci.openshift.org"
    get_release: true
    
    ## OCP cluster installtion
    cluster_name: "my-ocp-cluster"
    cluster_dir:  "{{ base_dir }}/{{ cluster_name }}"
    install_type: "gcp"
    
    # AWS specific
    aws_domain: "devcluster.openshift.com"
    aws_region: "us-east-1"
    # GCP specific
    gcp_domain: "gcp.devcluster.openshift.com"
    gcp_project_id: "openshift-gce-devel"
    gcp_region: "us-east4"
    
    # DNS Domain, pull secret, and ssh key paths
    dns_domain: "{{ gcp_domain }}"
    pull_secret_file: "~/.gcp/pull-secret.json"
    ssh_key_file: "~/.ssh/id_rsa.pub"
    ssh_key: "{{ lookup('file', ssh_key_file) }}"
    create_cluster: true
    kubeadmin: "kubeadmin"
    
    ## OpenShift Pipelines operator and tkn cli
    op_name: "openshift-pipelines-operator-rh"
    op_source: "redhat-operators"
    op_channel: "preview"
    tkn_version: "0.9.0"
```

## ansible roles

- install_prereqs - Install prereq binaries - go, kubectl, jq, and yq  
- install_ocp_bins - Install OpenShift installer and client
- install_cluster - Install an OpenShift cluster on GCP or AWS providing your own pullsecret and ssh key
- install_tekton - Install OpenShift Pipelines/Tektoncd Pipelines

## Examples

### Install cluster in GCP and openshift pipelines (tekton)
```
ansible-playbook -vv -i "localhost," -c local -e cluster_name=my-tekton-cluster \
    ./playbooks/install.yml
```

### Install cluster in GCP only
```
ansible-playbook -vv -i "localhost," -c local -e cluster_name=my-tekton-cluster \
    -e install_tekton=false 
    ./playbooks/install.yml
```

### Install cluster only in GCP with ci version 4.3 of OCP
```
ansible-playbook -vv -i "localhost," -c local -e cluster_name=my-tekton-cluster \
    -e ocp_ci_release=4.3.0-0.ci \
    -e get_release=false \
    -e install_tekton=false \
    ./playbooks/install.yml
```

### Install prereqs and tekton only on some cluster with a different dns_domain
```
ansible-playbook -vv -i "localhost," -c local -e cluster_name=cptektonari \
    -e install_cluster=false -e install_tekton=true -e dns_domain=lab.clus2.redhat.com \
    ./playbooks/install.yml
```

## Outputs
 
At the end of a run the console url, API url, and the kubeadmin_password are printed.  </br>
They Are also stored in the cluster name directory in the cluster_creds.log.