# Buildkite Setup

Create a Buildkite pipeline linked to `AXDOOMER/AKSFlexNode` in the same cluster used by
`tgrep`. Buildkite checks out AKSFlexNode using its GitHub integration; AKSFlexNode is public and
does not need a GitHub token.

The `GITLAB_TOKEN` cluster secret (already created for `tgrep`) grants access to
the private
`arctiq-team/alexandrelabonte/ci-pipeline-generation/ci-pipeline-generator`
repository. Buildkite injects it into the bootstrap job environment, which uses
it through `GIT_ASKPASS` to authenticate the GitLab HTTPS clone, builds the
generator in `golang:1.25`, and passes it into that Docker container before
uploading its Buildkite output as a dynamic pipeline.
The doubled dollar signs in `pipeline.yml` preserve shell variables until the
Buildkite job runs.

The generated Go jobs run in `golang:latest`, so they do not require Go to be
preinstalled on the agent.
