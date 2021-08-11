# otel_jaeger
This repo consists k8s templates to create otel collector along with Jaeger prod setup

The Jaeger Operator is an implementation of a Kubernetes Operator.

Getting started
Firstly, ensure an ingress-controller is deployed. When using minikube, you can use the ingress add-on: minikube start --addons=ingress

To install the operator, run:

kubectl create namespace observability
kubectl create -n observability -f https://raw.githubusercontent.com/jaegertracing/jaeger-operator/master/deploy/crds/jaegertracing.io_jaegers_crd.yaml
kubectl create -n observability -f https://raw.githubusercontent.com/jaegertracing/jaeger-operator/master/deploy/service_account.yaml
kubectl create -n observability -f https://raw.githubusercontent.com/jaegertracing/jaeger-operator/master/deploy/role.yaml
kubectl create -n observability -f https://raw.githubusercontent.com/jaegertracing/jaeger-operator/master/deploy/role_binding.yaml
kubectl create -n observability -f https://raw.githubusercontent.com/jaegertracing/jaeger-operator/master/deploy/operator.yaml

Once the jaeger-operator deployment in the namespace observability is ready, create a Jaeger instance, like:

kubectl apply -n observability -f https://raw.githubusercontent.com/Shashank852/otel_jaeger/main/jaeger_simple_prod.yaml

This will create a Jaeger instance named simplest. The Jaeger UI is served via the Ingress, like:

$ kubectl get -n observability ingress
NAME             HOSTS     ADDRESS          PORTS     AGE
simplest-query   *         192.168.122.34   80        3m
In this example, the Jaeger UI is available at http://192.168.122.34.

The official documentation for the Jaeger Operator, including all its customization options, are available under the main Jaeger Documentation.

Compatibility matrix
The following table shows the compatibility of jaeger operator with different components, in this particular case we shows Kubernetes and Strimzi operator compatibility

Jaeger Operator	Kubernetes	Strimzi Operator
v1.24	v1.19, v1.20, v1.21	v0.23
v1.23	v1.19, v1.20, v1.21	v0.19, v0.20
v1.22	v1.18 to v1.20	v0.19
Jaeger Operator vs. Jaeger
The Jaeger Operator follows the same versioning as the operand (Jaeger) up to the minor part of the version. For example, the Jaeger Operator v1.22.2 tracks Jaeger 1.22.0. The patch part of the version indicates the patch level of the operator itself, not that of Jaeger. Whenever a new patch version is released for Jaeger, we'll release a new patch version of the operator.

Jaeger Operator vs. Kubernetes
We strive to be compatible with the widest range of Kubernetes versions as possible, but some changes to Kubernetes itself require us to break compatibility with older Kubernetes versions, be it because of code imcompatibilities, or in the name of maintainability.

For instance, when we released v1.22.0, the latest Kubernetes version was v1.20.5. As such, the minimum version of Kubernetes we support for Jaeger Operator v1.22.0 is v1.18 and we tested it with up to 1.20.

The Jaeger Operator might work on versions outside of the given range, but when opening new issues, please make sure to test your scenario on a supported version.
