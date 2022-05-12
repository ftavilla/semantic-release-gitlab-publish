# semantic-release-npm-gitlab-publish

[**semantic-release**](https://github.com/semantic-release/semantic-release) shareable config to publish npm packages with [Gitlab](https://gitlab.com).


## Plugins

This [shareable configuration](https://github.com/ftavilla/semantic-release-npm-gitlab-publish/blob/master/src/gitlab/.releaserc.json) uses the following plugins:

- [`@semantic-release/commit-analyzer`](https://github.com/semantic-release/commit-analyzer)
- [`@semantic-release/release-notes-generator`](https://github.com/semantic-release/release-notes-generator)
- [`@semantic-release/changelog`](https://github.com/semantic-release/changelog)
- [`@semantic-release/gitlab`](https://github.com/semantic-release/gitlab)
- [`@semantic-release/npm`](https://github.com/semantic-release/npm)
- [`@semantic-release/git`](https://github.com/semantic-release/git)

## Summary

- Provides an informative [git](https://github.com/semantic-release/git) commit message for the release commit that does not trigger continuous integration and conforms to the [conventional commits specification](https://www.conventionalcommits.org/) (e.g., "chore(release): 1.2.3 [skip ci]").
- Produce a release note [release-notes-generator](https://github.com/semantic-release/release-notes-generator).
- Creates a tarball that gets uploaded with each [Gitlab release](https://github.com/semantic-release/gitlab).
- Publishes the same tarball to [Gitlab registry](https://github.com/semantic-release/npm).
- Commits the version change in `package.json`.
- Creates or updates a [changelog](https://github.com/semantic-release/changelog) file in the docs directory.

## Install

```bash
$ npm install --save-dev semantic-release semantic-release-npm-gitlab-publish
```

## Usage

The shareable config can be configured in the [**semantic-release** configuration file](https://github.com/semantic-release/semantic-release/blob/master/docs/usage/configuration.md#configuration):

```json
{
  "extends": "semantic-release-npm-gitlab-publish",
  "branch": "master",
  "preset": "angular"
}
```

## Configuration

Ensure that your CI configuration has the following **_secret_** environment variables set:
- [`GL_TOKEN`](https://github.com/semantic-release/gitlab#configuration) with `api` and `write_repository` scopes as mention in the 

See the documentation for required installation and configuration steps.

### Gitlab CI configuration

Here is an example of what your gitlab-ci.yaml `publish` job should look like:

```yml
name: CICD

stages:
  - publish

.base:
  image: node:16
  
publish:
  stage: publish
  extends:
    - .base
  environment:
    name: PROD
  only:
    - master
  script:
    - npx semantic-release
```