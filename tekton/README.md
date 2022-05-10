- [Tekton pipeline 和 Tekton dashboard](#tekton-pipeline-和-tekton-dashboard)
  - [安装pipeline](#安装pipeline)
    - [task 验证](#task-验证)
      - [1.创建第一个task](#1创建第一个task)
      - [2.创建第一个task run](#2创建第一个task-run)
      - [3.验证工作正常](#3验证工作正常)
      - [4.查询task run 日志](#4查询task-run-日志)
    - [pipeline 验证](#pipeline-验证)
      - [1.创建第二个task](#1创建第二个task)
      - [2.创建pipeline](#2创建pipeline)
      - [3.创建pipeline run](#3创建pipeline-run)
      - [4.查询pipeline run 日志](#4查询pipeline-run-日志)
  - [安装dashboard](#安装dashboard)
    - [访问](#访问)
  - [清理](#清理)
  - [参考](#参考)

# Tekton pipeline 和 Tekton dashboard

## 安装pipeline
```
kubectl apply -f tekton-pipeline.yaml
```
pod 状态 Running为正常
```
kubectl get pods -n tekton-pipelines -w
```
### task 验证
#### 1.创建第一个task
```
kubectl apply -f hello-world.yaml
```

#### 2.创建第一个task run 
```
kubectl apply -f hello-world-run.yaml
```

#### 3.验证工作正常
```
kubectl get taskrun hello-task-run
```

输出：

|NAME|             SUCCEEDED|   REASON|      STARTTIME|   COMPLETIONTIME|
| ----------- | ----------- |----------- |----------- |----------- |
|hello-task-run|   True|        Succeeded|   8d|          8d|

#### 4.查询task run 日志
```
kubectl logs --selector=tekton.dev/taskRun=hello-task-run
```

输出：

```
Hello World
```

### pipeline 验证
#### 1.创建第二个task
```
kubectl apply -f goodbye-world.yaml
```

#### 2.创建pipeline
```
kubectl apply -f hello-goodbye-pipeline.yaml
```

#### 3.创建pipeline run
```
kubectl apply -f hello-goodbye-pipeline-run.yaml
```

#### 4.查询pipeline run 日志
```
kubectl logs --selector=tekton.dev/pipelineRun=hello-goodbye-run
```

输出：
```
[hello : hello] Hello World!

[goodbye : goodbye] Goodbye World!
```

## 安装dashboard
```
kubectl apply -f tekton-dashboard.yaml
```
pod 状态 Running为正常
```
kubectl get pods -n tekton-pipelines -w
```

### 访问
```
kubectl -n tekton-pipelines port-forward svc/tekton-dashboard 9097:9097
```

访问ip:9097 

如果本地localhost:9097

## 清理
```
kubectl delete -k .
```

## 参考
1. https://tekton.dev/docs/getting-started/tasks/
2. https://tekton.dev/docs/getting-started/pipelines/
3. https://tekton.dev/docs/dashboard/
