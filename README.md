# ElasticHQ on OpenShift

In this repository you'll find a ready to use [ElasticHQ](https://github.com/ElasticHQ/elasticsearch-HQ) application to deploy in your OpenShift cluster for internal ElasticSearch monitoring.

## How to use

1. Import all required images to your `logging` namespace (make sure that your cluster have access to docker public registry):

```shell
oc import-image nginx-114-rhel7 --from=registry.access.redhat.com/rhscl/nginx-114-rhel7 -n logging --confirm
oc import-image elastic-hq-proxy --from=elastichq/elasticsearch-hq -n logging --confirm
``` 

2. Clone this repository

```shell
git clone https://github.com/ricardozanini/elastic-hq-openshift.git
```

3. Process and create the application inside your `logging` namespace. Your cluster should have access to the Github. If not, just import this repository to your internal one and parameterize the template.

```shell
oc project logging
oc process -f elastic-hq-openshift/openshift/elastic-hq-template.yaml | oc process -f -
```

Without Github access:

```shell
oc project logging
oc process -f elastic-hq-openshift/openshift/elastic-hq-template.yaml PROXY_GIT_REPO=<your-repo-url> | oc process -f -
```

4. A new build should start, otherwise just run

```shell
oc start-build elastic-hq-proxy
```

