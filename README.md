<img src="https://camo.githubusercontent.com/5b298bf6b0596795602bd771c5bddbb963e83e0f/68747470733a2f2f692e696d6775722e636f6d2f7031527a586a512e706e67" align="left" width="144px" height="144px"/>

### My home Kubernetes cluster :sailboat:
_... managed by Flux and serviced with RenovateBot_ :robot:

<br/>

## :book:&nbsp; Overview

This repository _is_ my homelab Kubernetes cluster in a declarative state. [Flux2](https://github.com/fluxcd/flux2) watches my [cluster](./cluster/) folder and makes the changes to my cluster based on the YAML manifests.

Feel free to open a [Github issue](https://github.com/onedr0p/k3s-gitops/issues/new) or join the k8s@home [Discord](https://discord.gg/DNCynrJ) if you have any questions.

---

## :computer:&nbsp; Cluster setup

My cluster is [k3s](https://k3s.io/) provisioned overtop Ubuntu 20.10 using the [Ansible](https://www.ansible.com/) galaxy role [ansible-role-k3s](https://github.com/PyratLabs/ansible-role-k3s).

See my [server/ansible](./server/ansible/) directory for my playbooks and roles.

---

## :robot:&nbsp; Automate all the things!

- [Github Actions](https://docs.github.com/en/actions) for running automated Ansible tests and checking code formatting.
- Rancher [System Upgrade Controller](https://github.com/rancher/system-upgrade-controller) to apply updates to k3s
- [Renovate](https://github.com/renovatebot/renovate) with the help of the [k8s-at-home/renovate-helm-releases](https://github.com/k8s-at-home/renovate-helm-releases) Github action keeps my application charts and container images up-to-date
- [Actions Runner Controller](https://github.com/summerwind/actions-runner-controller) operates a self-hosted Github runner in my cluster that updates Sealed Secrets when needed
- ~~[Kured](https://github.com/weaveworks/kured) to apply OS patches to my nodes~~

---

---

## :globe_with_meridians:&nbsp; Networking

In my cluster I run [coredns](https://github.com/coredns/coredns), [etcd](https://github.com/etcd-io/etcd), and [external-dns](https://github.com/kubernetes-sigs/external-dns). **External-DNS** populates **CoreDNS** with all my ingress records and stores it in **etcd**. When I'm browsing any of the webapps while on my home network, the traffic is being routed internally and never makes a round trip. The way I set this up is in my router. When a DNS request is made for my domain or any of my subdomains it uses **coredns** as the DNS server, otherwise it uses whatever upstream DNS I provided.

---

## :wrench:&nbsp; Tools

| Tool                                                   | Purpose                                                                                                   |
|--------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| [direnv](https://github.com/direnv/direnv)             | Set `KUBECONFIG` environment variable based on present working directory                                  |
| [git-crypt](https://github.com/AGWA/git-crypt)         | Encrypt certain files in my repository that can only be decrypted with a key on my computers              |
| [go-task](https://github.com/go-task/task)             | Replacement for make and makefiles, who honestly likes that?                                              |
| [pre-commit](https://github.com/pre-commit/pre-commit) | Ensure the YAML and shell script in my repo are consistent                                                |
| [kubetail](https://github.com/johanhaleby/kubetail)    | Tail logs in Kubernetes, also check out [stern](https://github.com/wercker/stern) ([which fork? good luck](https://techgaun.github.io/active-forks/index.html#https://github.com/wercker/stern)) |

---

## :handshake:&nbsp; Thanks

A lot of inspiration for my cluster came from the people that have shared their clusters over at [awesome-home-kubernetes](https://github.com/k8s-at-home/awesome-home-kubernetes)
