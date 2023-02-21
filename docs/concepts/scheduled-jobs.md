(scheduled-jobs)=
# Scheduled jobs and workflows

Oftentimes you may want to run a `job` on a regular schedule. For example, fetching from a datasource every morning, compiling an analytics report every month, or detecting model drift every hour.

## Creating a job and scheduling it

MLRun makes it very simple to add a schedule to a given `job`. To showcase this, the following job runs the code below, which resides in a file titled `schedule.py`:

```python
def hello(context):
    print("You just ran a scheduled job!")
```

To create the job, use the `code_to_function` syntax and specify the `kind` like below:

```python
import mlrun

job = mlrun.code_to_function(
    name="my-scheduled-job",      # Name of the job (displayed in console and UI)
    filename="schedule.py",       # Python file or Jupyter notebook to run
    kind="job",                   # Run as a job
    image="mlrun/mlrun",          # Use this Docker image
    handler="hello"               # Execute the function hello() within code.py
)
```

**Running the job using a schedule**

To add a schedule, run the job and specify the `schedule` parameter using Cron syntax like so:

```python
job.run(schedule="0 * * * *")
```

This runs the job every hour. An excellent resource for generating Cron schedules is [Crontab.guru](https://crontab.guru/).

## Scheduling a workflow

After loading a project (`load_project`) from a remote source e.g GitHub, that contains functions and workflow (workflow named main).
run the project with the scheduled workflow:
```
project.run("main", schedule='0 * * * *')
```
For Deleting exciting scheduled workflow you can run the example below, or delete it from the UI:
```
project.run("main", schedule='0 * * * *',override=True)
```
```{admonition} Note
1. Remote workflows can only be performed by a project with **remote** source (git://github.com/mlrun/something.git, http://some/url/file.zip or http://some/url/file.tar.gz).
    * To set project source use the `project.set_source` method.
    * To set workflow use the `project.set_workflow` method.
3. Example for a remote GitHub project - https://github.com/mlrun/project-demo
```

