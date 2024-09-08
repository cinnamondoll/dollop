# prep steps

install kubectl and friends to `/usr/local/bin` on all `hosts`:

    ansible-playbook -i hosts tools.yml --ask-become-pass

install deps for longhorn storage on `[servers]` only:

    ansible-playbook -i hosts deps.yml

install k0s cluster:

    k0sctl apply

fetch kubeconfig:

    mkdir -p $HOME/.kube
    k0sctl kubeconfig > $HOME/.kube/config

populate environment with our `versions.yml` vars:

    eval $(awk -F ": " '{print $1"="$2}' versions.yml)

install argocd and patch it:

    kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-cd/$argocd_version/manifests/core-install.yaml
    kubectl patch clusterrolebinding argocd-application-controller --type='merge' -p "$(cat argocd.patch.yml)"

add this repo's `/manifests/` folder as a argocd app, recursively applying everything in it as a argocd app too:

    kubectl apply -f manifests.yml

