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
First of all, thanks for considering to contribute 🎉 
  
Follow the next steps to start developing:   
1. [Fork this repository](https://gitlab.com/commonground/haven/havenplus/gitops-flux/-/forks/new)
2. Copy `.env.example` to `.env` and add the URL of the fork you've just created. In Gitlab, this should be the URL under the `Code` button which states `Clone with HTTPS`.
3. Run a local cluster by following the instructions from the previous chapter. This will bootstrap FluxCD based on your fork. 
4. Make your changes in the fork; FluxCD will automatically pick-up these changes. 
5. Once ready, create a Merge Request with the upstream repository as a target. 

> If you're a member of this GitLab project with at least developer permissions, you can skip the fork workflow. Simply create a branch from main and configure it in your .env file.

## Get Involved
* Ask your question by [creating an issue](https://gitlab.com/commonground/haven/havenplus/gitops-flux/-/issues/new)
* Join us in [Digilab's Mattermost](https://digilab.overheid.nl/chat/digilab/channels/havenplus)

## License
Licensed under the [EUPL](LICENSE.md).