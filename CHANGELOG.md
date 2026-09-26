# Changelog

## Unreleased

### Changed

* Upgraded `hashicorp/google` from 3.68.0 to 8.4.0, `hashicorp/random` from 3.1.0 to 3.9.1 and `chriskuchin/hiera5` from 0.3.0 to 0.5.4.
* Raised `required_version` to `>= 1.5`, covering OpenTofu 1.x and Terraform 1.x.
* Ran `tofu fmt` across the module. The only non-whitespace changes are quoted variable names and `map` written as `map(any)`, which is the same type.

### Fixed

* hiera5 0.4.0 and later look for `hiera.yml` when `config` is not set, where 0.3.0 looked for `hiera.yaml`. The provider block now sets `config = "${path.module}/hiera.yaml"`. Without it every sizing lookup fails with "key not found".
* google 6.0 changed the default `balancing_mode` on `google_compute_region_backend_service` backends from `CONNECTION` to `UTILIZATION`. Passthrough load balancers, which is what the compiler pool uses, need `CONNECTION`, so it is now set explicitly.
* google 6.0 also changed the default `connection_draining_timeout_sec` on the same resource from 0 to 300. It is now pinned to 0 to keep the previous behaviour.

### Behaviour to be aware of

* google 5.0 made `labels` non-authoritative and added `terraform_labels` and `effective_labels`. The first plan against state from 3.x will show label fields being filled in.
* google 6.0 adds a `goog-terraform-provisioned = true` label to newly created resources. The module leaves this default on.
* Input variables, the `console` and `pool` outputs, and the `google_compute_instance` resource names are unchanged.
