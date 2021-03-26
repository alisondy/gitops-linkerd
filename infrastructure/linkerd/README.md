# Generate Linkerd v2 certificates

Install the step CLI on MacOS and Linux using Homebrew run:

```sh
brew install step
```

Generate the Linkerd trust anchor certificate:

```sh
step certificate create root.linkerd.cluster.local ca.crt ca.key \
--profile root-ca --no-password \
--insecure
```

Generate the Linkerd issuer certificate and key:

```sh
step certificate create identity.linkerd.cluster.local issuer.crt issuer.key \
--profile intermediate-ca --not-after 8760h --no-password --insecure \
--ca ca.crt --ca-key ca.key
```

Generate a Kubernetes secret with the Linkerd certs:

```sh
kubectl -n linkerd create secret generic linkerd-certs \
--from-file=ca.crt --from-file=issuer.crt \
--from-file=issuer.key -oyaml --dry-run=client \
> linkerd-certs.yaml
```
