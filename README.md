# operator-subscriptions
Automatically install operators in Daily Clusters


```bash
echo "Installing Pipelines Operator"
oc apply -f https://raw.githubusercontent.com/openshift-dev-console/operator-subscriptions/main/pipelines-subscription.yaml
echo "Installing Serverless Operator"
oc create ns openshift-serverless
oc apply -f https://raw.githubusercontent.com/openshift-dev-console/operator-subscriptions/main/serverless-subscription.yaml
oc wait subscriptions.operators.coreos.com --for=condition=CatalogSourcesUnhealthy=false --timeout=20m -n openshift-serverless serverless-operator
sleep 120
oc apply -f https://raw.githubusercontent.com/openshift-dev-console/operator-subscriptions/main/knative-serving.yaml -n knative-serving
oc wait KnativeServing --for=condition=Ready --timeout=20m -n knative-serving knative-serving
echo "Installing Builds Operator"
oc apply -f https://raw.githubusercontent.com/openshift-dev-console/operator-subscriptions/main/builds-subscription.yaml
oc wait subscriptions.operators.coreos.com --for=condition=CatalogSourcesUnhealthy=false --timeout=20m -n openshift-operators openshift-builds-operator
sleep 20
oc apply -f https://raw.githubusercontent.com/openshift-dev-console/operator-subscriptions/main/shipwright-build.yaml
oc wait ShipwrightBuild --for=condition=Ready --timeout=20m -n openshift-builds openshift-builds        
oc apply -f https://github.com/redhat-developer/openshift-builds-catalog/raw/main/clusterBuildStrategy/buildah/buildah.yaml
oc apply -f https://github.com/redhat-developer/openshift-builds-catalog/raw/main/clusterBuildStrategy/source-to-image/source_to_image.yaml
oc apply -f https://github.com/redhat-developer/openshift-builds-catalog/raw/main/clusterBuildStrategy/buildpacks/buildpacks.yaml
```
