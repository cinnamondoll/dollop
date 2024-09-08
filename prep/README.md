# prep steps

install kubectl and friends to `/usr/local/bin` on all `hosts`:

    ansible-playbook -i hosts tools.yml --ask-become-pass

install k0s cluster:

    k0sctl apply

fetch kubeconfig:

    mkdir -p $HOME/.kube
    k0sctl kubeconfig > $HOME/.kube/config

populate environment with our `versions.yml` vars:

    eval $(awk -F ": " '{print $1"="$2}' versions.yml)

install argocd and connect it to our repo:

    kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-cd/$argocd_version/manifests/core-install.yaml
    kubectl apply -f manifests.yml
