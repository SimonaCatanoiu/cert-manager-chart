# Cert Manager Helm Chart
Helm umbrella chart to deploy Cert-Manager and a Cloudflare-backed `ClusterIssuer`.

## Quick start
```sh
make install
```
Installs release `cert-manager` into namespace `cert-manager` using values from [chart/values.yaml](chart/values.yaml).

## Common operations
- Install: `make install`
- Upgrade: `make upgrade`
- Uninstall: `make uninstall`
- Render templates locally: `make template`

## Configuration
Edit [chart/values.yaml](chart/values.yaml):
- `crds.enabled`: set to `true` to install the cert-manager CRDs with the release.
- `crds.keep`: set to `true` to keep CRDs on uninstall.
- `acme.email`: email used for ACME account registration.
- `cloudflare.apiToken`: set the CloudFlare API Token needed for the DNS01-Challenge 

### ClusterIssuer
[chart/templates/clusterissuer.yaml](chart/templates/clusterissuer.yaml) defines an ACME issuer that uses the Cloudflare DNS01 solver. Ensure the secret name matches the one created above and that `acme.email` is set.

## Maintenance
- Update dependencies: `make deps`
- Lint chart: `make lint`
- YAML lint (containerized): `make yamllint`
- YAML format (containerized): `make yamlfix`
- Generate Helm docs (containerized): `make helm-docs`
