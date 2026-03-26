## 1. How did you test your pipelines?

The pipelines were tested directly in Jenkins running on Kubernetes (Minikube using Docker).

### Steps:
- Created **PipelineB** and **PipelineC** using *Pipeline script from SCM*
- Triggered builds manually using **"Build Now"**
- Monitored execution using:
  - Jenkins Console Output
  - Kubernetes pods:
    ```bash
    kubectl get pods -n jenkins -w
    ```

### Verification:
- Agent pod was created dynamically for each build
- All stages executed successfully
- Artifacts were generated:
  - **PipelineB** → `doc.tar.gz`
  - **PipelineC** → `doc.tar.gz`, `doxygen_warnings_parsed.csv`

---

## 2. How did you test repoC python?

- Executed **PipelineC** in Jenkins to generate a real `doxygen_warnings.log` using Doxygen
- While the pipeline was running, accessed the pod:
  ```bash
  kubectl exec -it <jenkins-agent-pod> -- /bin/bash
  cd /home/jenkins/agent/workspace/PipelineC
  cat doxygen_warnings.log ```
- Download the content file in the Jenkins build artifact for each build and check the file `doxygen_warnings_parsed.csv`

## 3. What is the advantage to use LFS?
- Handles large files efficiently
- Keeps Git repo lightweight