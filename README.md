# Cert Manager Helm Chart
Helm umbrella chart to deploy Cert-Manager and a Cloudflare-backed `ClusterIssuer` for Let's Encrypt certificate automation via DNS01 ACME challenges.

## Features
- Automatic deployment of cert-manager with Let's Encrypt integration
- Cloudflare DNS01 ACME solver for wildcard certificate support
- Separation of CRD management for better lifecycle control
- Configurable email for Let's Encrypt registration
- Secure API token management via Kubernetes Secrets
- Container-based tooling for linting and documentation

## Quick start
```sh
make install
```
Installs release `cert-manager` into namespace `cert-manager`, applies cert-manager CRDs, and creates a ClusterIssuer configured with Cloudflare DNS01 solver.

## Prerequisites
- Kubernetes cluster with kubectl configured
- Helm 3.x installed
- Cloudflare account with API token
- Valid email address for Let's Encrypt registration

## Common operations
- Install: `make install` - Install cert-manager release with CRDs and ClusterIssuer
- Upgrade: `make upgrade` - Upgrade or reinstall cert-manager release
- Uninstall: `make uninstall` - Remove cert-manager release
- Render templates locally: `make template` - Render Helm templates without applying
- Install CRDs only: `make install-crds` - Install cert-manager Custom Resource Definitions
- Uninstall CRDs only: `make uninstall-crds` - Remove cert-manager CRDs

## Configuration
Edit [chart/values.yaml](chart/values.yaml) to customize the deployment:
- `acme.email`: Email address used for Let's Encrypt account registration (required)
- `cloudflare.apiToken`: Cloudflare API token for DNS01 challenge (required)
- `cert-manager.enabled`: Enable/disable cert-manager subchart (default: `true`)
- `cert-manager.crds.enabled`: Set to `false` to manage CRDs separately (default: `false`)
- `cert-manager.crds.keep`: Keep CRDs on uninstall (default: `true`)

### Created Resources
The chart creates the following resources:

#### Secret: cloudflare-api-token-secret
[chart/templates/cloudflare-api-token-secret.yaml](chart/templates/cloudflare-api-token-secret.yaml) - Stores the base64-encoded Cloudflare API token in the `cert-manager` namespace.

#### ClusterIssuer: letsencrypt-cloudflare
[chart/templates/clusterissuer.yaml](chart/templates/clusterissuer.yaml) - Defines an ACME ClusterIssuer that uses Let's Encrypt with Cloudflare DNS01 solver for domain validation. This issuer can be referenced by Certificate resources across the cluster.

## Development & Maintenance
- Update dependencies: `make deps` - Update Helm chart dependencies from registered repos
- Lint chart: `make lint` - Validate Helm chart syntax
- Lint YAML: `make yamllint` - Run yamllint in container for YAML style validation
- Format YAML: `make yamlfix` - Auto-format YAML files in container
- Generate docs: `make helm-docs` - Generate Helm chart documentation
- Help: `make help` - Display all available Makefile targets
