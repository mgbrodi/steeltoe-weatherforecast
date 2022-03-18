SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='harbor.marygabry.name/library/weatherforecast-steeltoe')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'weatherforecast-steeltoe',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
              " --local-path " + LOCAL_PATH +
              " --source-image " + SOURCE_IMAGE +
              " --namespace " + NAMESPACE +
              " --yes >/dev/null" +
              " && kubectl get workload weatherforecast-steeltoe --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['*.cs', './Controllers'],
    container_selector='workload'
)

k8s_resource('weatherforecast-steeltoe', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'weatherforecast-steeltoe'}])
