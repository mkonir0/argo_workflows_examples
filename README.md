execute argo in minikube 
```
ARGO_WORKFLOWS_VERSION="vX.Y.Z"  
kubectl create namespace argo  
kubectl apply -n argo -f "https://github.com/argoproj/argo-workflows/releases/download/${ARGO_WORKFLOWS_VERSION}/quick-start-minimal.yaml"  
kubectl -n argo port-forward service/argo-server 2746:2746
```

```
argo submit -n argo hello-world.yaml                        # create and trigger Workflow  
argo submit -n argo parameter.yaml -p team="red" --watch    # create and trigger Workflow with parameter
argo cron create -n argo cron.yaml                          # create a CronWorkflow
argo list -n argo                                           # list current workflows
argo get hello-world-xxx -n argo                            # get info about a specific workflow
argo logs hello-world-xxx -n argo                           # print the logs from a workflow
argo delete hello-world-xxx -n argo                         # delete workflow

argo template create # create template resource
```

--- it's also possible to use kubectl to update resources ( argo is just a CRD ), but argo cli executes that for you
```
kubectl -n argo apply -f simple.yaml
```


https://argo-workflows.readthedocs.io/en/latest/workflow-concepts/#script

Core K8 resource types  (Kind:xxx)
* Workflow
* WorkflowTemplate 
* CronWorkflows


* Sensor - event-driven workflow, sensors are responsible for watching for specific events from various sources and triggering workflows based on those events.
---

Template types
* Container
* Script
* Resource = operation inside cluster - eg.configmap
* Suspend = suspend executionapiVersion: argoproj.io/v1alpha1
 
 Template invocators - invoke/call other templates and provide execution control
* Steps - seznam stepu
* DAG -  graph of dependencies


Rozdíl mezi Workflow a CronWorkflow

Workflow: Jednorázový běh, který je spuštěn manuálně pomocí příkazu argo submit.
Tento typ workflowu se vykoná jednou v okamžiku jeho nasazení.

CronWorkflow: Opakovaný (plánovaný) běh, který je spuštěn automaticky na základě definovaného časového plánu,
podobně jako cron úloha v Linuxu. CronWorkflow je spravován Kubernetes a nasazuje nové instance Workflow dle plánu.