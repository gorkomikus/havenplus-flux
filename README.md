# GitOps-FluxCD Reference Implementation 

## Getting Started 
The following instructions will spin up a local [Kind](https://kind.sigs.k8s.io/) cluster, install FluxCD and let it deploy all workloads defined in this repository. 

### Install dependencies / required tools
Use [asdf](https://asdf-vm.com/guide/getting-started.html) to ensure the correct versions of dependencies/tools.
```bash
# in the root of this project:
awk '{ system("asdf plugin add " $1) }' < .tool-versions
asdf install
```

### Run Locally
The following command will spin up a local Kind cluster, install FluxCD and let it deploy all workloads defined in this repository: 
```bash
task run-local
```

Once this command has completed successfully, you can check the status of the deployed workloads as follows:
```bash
# show all Flux objects
flux get all

# verify pods in the cert-manager namespace
kubectl -n cert-manager get pods
```

To delete your local cluster, run:
```bash
task uninstall
```

## Contributing
Merge Requests are more than welcome! Please consult our our [CONTRIBUTING guide](./CONTRIBUTING.md) for more information.

## Get Involved
* Ask your question by [creating an issue](https://gitlab.com/commonground/haven/havenplus/gitops-flux/-/issues/new)
* Join us in [Digilab's Mattermost](https://digilab.overheid.nl/chat/haven/channels/town-square)

## License
Licensed under the [EUPL](LICENSE.md).