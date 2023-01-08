# EKS deployments with Helm

GitHub action for deploying to AWS EKS clusters using helm.

This Action deploys using helm in private repository accessible by user name and password

Note:  If your EKS cluster administrative access is in a private network, you will need to use a self hosted runner in that network to use this action.

## Customizing

### Inputs

Following inputs can be used as `step.with` keys

| Name                    | Type   | Description                                                                                                                                               |
| ----------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `aws-secret-access-key` | String | AWS secret access key part of the aws credentials. This is used to login to EKS.                                                                          |
| `aws-access-key-id`     | String | AWS access key id part of the aws credentials. This is used to login to EKS.                                                                              |
| `aws-region`            | String | AWS region to use. This must match the region your desired cluster lies in.                                                                               |
| `cluster-name`          | String | The name of the desired cluster.                                                                                                                          |
| `cluster-role-arn`      | String | If you wish to assume an admin role, provide the role arn here to login as.                                                                               |
| `action`                | String | Determines if we `install` or `uninstall` the chart. (Optional, Defaults to `install`)                                                                    |
| `config-files`          | String | Comma separated list of helm values files.                                                                                                                |
| `namespace`             | String | Kubernetes namespace to use.  Will create if it does not exist                                                                                            |
| `values`                | String | Comma separated list of value set for helms. e.x:`key1=value1,key2=value2`                                                                                |
| `name`                  | String | The name of the helm release                                                                                                                              |
| `chart-path`            | String | The path to the chart. (defaults to `helm/`)                                                                                                              |
| `chart-repository`      | String | The URL of the chart-repository (Optional)                                                                                                                |
| `version`               | String | The version of the chart (Optional)                                                                                                                      |
| `plugins`               | String | Comma separated list of plugins to install. e.x:` https://github.com/hypnoglow/helm-s3.git, https://github.com/someuser/helm-plugin.git` (defaults to none) |
| `timeout`               | String | The value of the timeout for the helm release                                                                                                             |
| `update-deps`           | String | Update chart dependencies                                                                                                                                 |
| `helm-wait`             | String | Add the helm --wait flag to the helm Release (Optional)                                                                                                   |
| `atomic`                | String | Add the helm --atomic flag if set (Optional)                                                                                                              |
| `username`                | String | Add the helm --username flag if set (Optional)                                                                                                              |
| `password`                | String | Add the helm --password flag if set (Optional)                                                                                                              |

## Example usage

```yaml
uses: vijaysarthy01/github-action-helm-eks@v1.0.3
with:
  aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
  aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
  aws-region: us-west-2
  cluster-name: mycluster
  config-files: .github/values/dev.yaml
  chart-path: chart/
  namespace: dev
  values: key1=value1,key2=value2
  username: ${{ secrets.CD_JF_USER }}
  password: ${{ secrets.CD_JF_PASSWORD }}
  name: release_name
```

## Example 2
```yaml
    - name: Deploy Helm
      uses: vijaysarthy01/github-action-helm-eks@v1.0.3
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-west-2
        cluster-name: mycluster
        cluster-role-arn: ${{ secrets.AWS_ROLE_ARN }}
        config-files: fluent-bit/prod/values.yaml
        chart-path: fluent/fluent-bit
        namespace: logging
        name: fluent-bit
        chart-repository: https://fluent.github.io/helm-charts
        version: 0.20.6
        atomic: true
```

## Example Uninstall

```yaml
uses: vijaysarthy01/github-action-helm-eks@v1.0.3
with:
  aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
  aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
  aws-region: us-west-2
  action: uninstall
  cluster-name: mycluster
  namespace: dev
  name: release_name
```
