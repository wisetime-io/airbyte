## Setup

If you want, add the original repo as remote to fetch (potential) future changes.
Make sure you also disable push on the remote (as you are not allowed to push to it anyway).

```bash
git remote add upstream https://github.com/airbytehq/airbyte.git
git remote set-url --push upstream DISABLE
```

You can list all your remotes with `git remote -v`. You should see:
```
origin	git@github.com:wisetime-io/airbyte-builder.git (fetch)
origin	git@github.com:wisetime-io/airbyte-builder.git (push)
upstream	https://github.com/airbytehq/airbyte.git (fetch)
upstream	DISABLE (push)
```

> When you push, do so on `origin` with `git push origin`.
## Updates from upstream

> When you want to pull changes from `upstream` you can just fetch the remote and rebase on top of your work.
```bash
  git checkout master
  git fetch upstream
  git fetch upstream --tags
  git pull upstream
  git push origin
  ```
And solve the conflicts if any.

then rebase against version:
```
git checkout legebuild
git rebase --onto v0.50.30 origin/master
git push origin -f
```

## Build new image
```bash
./gradlew :airbyte-integrations:connectors:source-mssql:clean
./gradlew :airbyte-integrations:connectors:source-mssql:buildConnectorImage

docker tag airbyte/source-mssql:dev <new-image>
docker push <new-image>
```
