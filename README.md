# Drone Log Watcher

Bash script to monitor Drone CI logs in realtime.

The Drone GUI is not very good for viewing logs in real time. It's laggy, you 
have to manually switch to the next step when one completes, and often you
need to refesh the page in order to see the latest logs.

This script will take a repo name, and optionally a build number, then stream
the logs to stdout starting from the last running step.  Once the step is 
complete, will move on to the next step, and so on.

## Usage

    Usage: ./watch-drone repo [build]
    Watches all logs from a build of repo
    Arguments:
      repo: repository in group/project format.
      build (optional): build number to monitor, defaults to latest running build

## Examples

Watch logs of latest build:

    ./watch-drone foo/bar

Watch logs of specific build number:

    ./watch-drone foo/bar 123

## Caveats

* This does not work well with parallel pipelines or steps. When first run, it
  assumes that the last running step at the time is the one the user will be
  intestered in. This is because Drone API does not discriminate between normal
  steps and background services. Plus, with parallel steps, the script would
  not know which step or pipeline to use.

* For the same reason as above, race conditions can occur if services have 
  started but the first "proper" step hasn't.

* The build must be in "running" state to work. Pending or completed builds
  will not be detected.
