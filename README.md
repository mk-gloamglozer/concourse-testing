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

potential solutions

### master pipeline

have a per branch pipeline that is used for all branches, this then finds the pipeline files in that branch and creates/updates them using set_pipeline. The final step is to manually trigger each pipeline.

the trouble with this is that pipelines that should trigger because of inputs other than push to branch will still be triggered. also how should it name the pipelines as can this be done dynamically? (YES name can be dynamic, see commit 28a9454b2f60374791273d7074a5ec4665b920f4).

## how to stop pipeline running before pipeline is updated

can manually trigger created pipeline using the fly resource. https://github.com/troykinsella/concourse-fly-resource

could push back to <branch>/concourse and have the child pipeline listen for changes to that branch - bit convoluted.

using set_pipeline self does not update the pipeline before running. i.e. the pipeline will run using the old config and then be updated.

## OVERVIEW

Difficult to see how pipeline could be managed without a lot of manual work. With circle whatever is in the config file for that commit is used. With concourse the pipeline needs to be configured separately either by a separate pipeline or manually using the fly cli. If it is managed by a separate pipeline however the question arises how do we trigger the managed pipeline? If it is triggered by a git commit then it will run at the same time as the job that updates the pipeline, therefore the pipeline config will only take effect on the commit after the pipeline is updated.

### mitigation strategies

the management pipeline could trigger the child pipeline to run if it needs to be updated.

# conditional jobs (e.g. publish only if test succeeds)
