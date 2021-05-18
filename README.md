# helm-operator

[![Build Status](https://github.com/stackrox/helm-operator/workflows/CI/badge.svg?branch=main)

Experimental refactoring of the operator-framework's helm operator

## Why a fork?

Initially the Helm operator type was designed to automate Helm chart operations
by mapping the [values](https://helm.sh/docs/chart_template_guide/values_files/) of a Helm chart exactly to a 
`CustomResourceDefinition` and defining its watched resources in a `watches.yaml` 
[configuration](https://sdk.operatorframework.io/docs/building-operators/helm/tutorial/#watch-the-nginx-cr).

To write a [Level II+](https://sdk.operatorframework.io/docs/advanced-topics/operator-capabilities/operator-capabilities/) operator 
which reuses an already existing Helm chart a [hybrid](https://github.com/operator-framework/operator-sdk/issues/670)
between the Go and Helm operator type is necessary.

The hybrid approach allows to add customizations to the Helm operator like value mapping based on cluster state or
executing code in on specific events.

## Quickstart

Add this module as a replace directive to your `go.mod`:

```
go mod edit -replace=github.com/joelanford/helm-operator=github.com/stackrox/helm-operator@main
```

Example:

```
chart, err := loader.Load("path/to/chart")
if err != nil {
    panic(err)
}

reconciler := reconciler.New(
    reconciler.WithChart(*chart),
    reconciler.WithGroupVersionKind(gvk),
)

if err := reconciler.SetupWithManager(mgr); err != nil {
    panic(fmt.Sprintf("unable to create reconciler: %s", err))
}
```

