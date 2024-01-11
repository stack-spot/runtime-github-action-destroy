# runtime-destroy-action

[![Action test Ubuntu](https://github.com/stack-spot/runtime-github-action-ping/actions/workflows/action-test-ubuntu.yaml/badge.svg)](https://github.com/stack-spot/runtime-github-action-ping/actions/workflows/action-test-ubuntu.yaml)

GitHub action to run StackSpot Runtime Destroy Worker.

_**Note**: This action is supported on debian/RHEl like systems_

## 📚 Usage

### Requirements

### Use Case

Check how to implement the orchestration job using the [runtime-manager-action](https://github.com/stack-spot/runtime-manager-action)

```yaml
jobs:
  job1:
    runs-on: ubuntu-latest
    needs: [orchestration]
    strategy:
       matrix:
         task: ${{ fromJSON(needs.orchestration.outputs.tasks) }}
       fail-fast: true
       max-parallel: 1
    steps:
      - name: DESTROY
        if: contains( matrix.task.taskType , 'DESTROY')
        uses: stack-spot/runtime-destroy-action@v1
        with:
          FEATURES_LEVEL_LOG: debug
          CLIENT_ID: ${{ secrets.CLIENT_ID }}
          CLIENT_KEY: ${{ secrets.CLIENT_KEY }}
          CLIENT_REALM: ${{ secrets.CLIENT_REALM }}
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_SESSION_TOKEN: ${{ secrets.AWS_SESSION_TOKEN }}
          AWS_REGION: sa-east-1
          REPOSITORY_NAME: my-repository-name 
          RUN_TASK_ID: ${{ matrix.task.runTaskId }}
          CONTAINER_URL: my/container-url # not mandatory

```

* * *

## ▶️ Action Inputs

Field | Mandatory | Observation
------------ | ------------  | -------------
**CLIENT_ID** | YES | [StackSpot](https://stackspot.com/en/settings/access-token) Client ID.
**CLIENT_KEY** | YES | [StackSpot](https://stackspot.com/en/settings/access-token) Client KEY.
**CLIENT_REALM** | YES | [StackSpot](https://stackspot.com/en/settings/access-token) Client Realm.
**AWS_ACCESS_KEY_ID** | YES | [AWS](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html) Access Key ID
**AWS_SECRET_ACCESS_KEY** | YES | [AWS](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html) Secret Access Key
**AWS_SESSION_TOKEN** | YES | [AWS](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html) Session Token
**AWS_REGION** | YES | AWS region where resources with be provisioned. Used for tf backend as well (e.g: `us-east-1`).
**RUN_TASK_ID** | YES | StackSpot Runtime task id to be executed, according to [runtime-manager-action](https://github.com/stack-spot/runtime-manager-action).
**REPOSITORY_NAME** | YES | Repository name to checkout during task process.
**CONTAINER_URL** | NO | Container url reference (e.g `stackspot/image`)

* * *

## License

[Apache License 2.0](https://github.com/stack-spot/runtime-github-action-destroy/blob/main/LICENSE)
