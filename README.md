# Reusable GitHub Action Workflows

We provide a few workflows that you can use either directly with the `uses` directive, or use it inspiration for building your own.

Example usage with the `uses` directive:

```yaml
jobs:
  publish:
    name: Build
    uses: Mattilsynet/workflows/.github/workflows/gradle-build.yaml@1.0
    with:
      java_version: 11.0
      gradle_version: 7.4.2
      sonarcloud_project_id: Mattilsynet_acme-playground
    secrets:
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

Note the version number specification after the `@` sign. You should always use a version number, otherwise you will depend on master, which can break your workflows. You can find the list of versions on the releases page of this repository.

## Provided workflows:

### gradle-build
Uses gradle to build the project, and executes tests. If you provide a `sonarcloud_project_id` it will also push results to sonarcloud, see [the mapdocs page](https://map.mattilsynet.io/#/sonarcloud) for more information on this.

Example:
```yaml
name: Build project on pushes

on:
  push: ~
  workflow_dispatch: ~

jobs:
  publish:
    name: Build
    uses: Mattilsynet/workflows/.github/workflows/gradle-build.yaml@gradlebuild
    with:
      java_version: 11.0
      gradle_version: 7.4.2
      sonarcloud_project_id: Mattilsynet_acme-playground
    secrets:
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

### gradle-publish
Uses gradle to create a publication, and pushes it to the project's artifact repository. This works only when triggered by annotated tags in git, because it uses the git tag as the published version.

Example:

```yaml
name: Publish package to GitHub Packages
on:
  push:
    tags: ['*']

jobs:
  publish:
    name: Publish gradle package 
    uses: Mattilsynet/workflows/.github/workflows/gradle-build.yaml@1.0
    with:
      java_version: 11.0
      gradle_version: 7.4.2
      publish_version: ${{ github.ref_name }}
```
