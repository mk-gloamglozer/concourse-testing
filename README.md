# concourse-testing

# things to consider

## multiple git branches

using a template and allow dynamic pipeline creation
https://concourse-ci.org/multi-branch-workflows.html

## secret management

credential management in concourse
https://concourse-ci.org/creds.html

how to interpolate into pipelines
https://concourse-ci.org/vars.html

## how to handle changing concourse files

when a pipeline file is changed it must be uploaded to the concourse server in order to take effect.
when a new branch changes the config how do we go about ensuring that this is automatically done but that
this change only effects the current branch
