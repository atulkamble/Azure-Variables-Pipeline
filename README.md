# Azure-Variables-Pipeline
Sure Atul! Here’s a detailed overview with **examples of Azure Pipelines using variables** — including **pipeline-level**, **stage-level**, **group variables**, and **runtime parameters**.

---

## ✅ **1. Define Pipeline-Level Variables**

```yaml
trigger:
- main

variables:
  appName: 'mywebapp'
  environment: 'dev'
  buildConfig: 'Release'

pool:
  vmImage: 'ubuntu-latest'

steps:
- script: echo "Deploying $(appName) to $(environment) in $(buildConfig) mode"
```

---

## ✅ **2. Override Variables in Stages or Jobs**

```yaml
stages:
- stage: Build
  variables:
    environment: 'build-env'
  jobs:
  - job: BuildJob
    steps:
    - script: echo "Stage variable: $(environment)"

- stage: Deploy
  variables:
    environment: 'prod'
  jobs:
  - job: DeployJob
    steps:
    - script: echo "Deploying to $(environment)"
```

---

## ✅ **3. Use Variable Groups (from Library)**

```yaml
variables:
- group: "Common-Dev-Settings"

steps:
- script: echo "Value from variable group: $(mySecretVar)"
```

> Go to Azure DevOps > Pipelines > Library to create and manage variable groups (with secrets if needed).

---

## ✅ **4. Set Output Variables in Steps**

```yaml
steps:
- script: echo "##vso[task.setvariable variable=buildVersion]1.0.$(Build.BuildId)"
- script: echo "New build version is: $(buildVersion)"
```

---

## ✅ **5. Runtime Parameters (User Input)**

```yaml
parameters:
- name: runTests
  type: boolean
  default: true

steps:
- script: echo "Run tests: ${{ parameters.runTests }}"
```

Run the pipeline manually and select `true/false` at runtime.

---

## ✅ **6. Secrets as Variables**

```yaml
variables:
  - name: connectionString
    value: $(my-connection-string)   # From pipeline UI secret

steps:
- script: echo "Secret is not shown in logs"
```

> Secrets are automatically masked in logs.

---

## ✅ **7. Template Reusability with Variables**

### `build-template.yml`
```yaml
parameters:
- name: buildConfig
  default: 'Release'

steps:
- script: echo "Building in ${{ parameters.buildConfig }} mode"
```

### `azure-pipelines.yml`
```yaml
trigger: [main]

extends:
  template: build-template.yml
  parameters:
    buildConfig: 'Debug'
```

---

Would you like a **real use-case CI/CD pipeline** (e.g., for a Node.js, Python, or .NET app) using variables + stages + conditions?
