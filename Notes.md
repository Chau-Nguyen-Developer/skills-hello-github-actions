## Notes
* Workflows have jobs, and jobs have steps.  
* Each step consists of either a shell script that is executed, or a reference to an action that is run. When we talk about an action in this context, we mean a reusable unit of code.   
* An action is a pre-defined, reusable set of jobs or code that perform specific tasks within a workflow.

### Example of workflow
_//The name of this workflow_  
name: Post welcome comment  
_//Trigger this workflow only when a PR is opened (not edited or closed)_  
on:  
  pull_request:  
    types: [opened]  
  _//The workflow needs write permissions to comment on pull requests._  
  permissions:  
    pull-requests: write  
  _//Define a job called "build". The name could be anything. It is just a label._    
  jobs:  
    build:  
      name: Post welcome comment  
      _//The runner (virtual machine) will be Ubuntu Linux._  
      runs-on: ubuntu-latest  
     _//The steps inside the job._  
      steps:  
        _//This step runs a command that posts a comment saying "Welcome to the repository!"_  
        - run: gh pr comment $PR_URL --body "Welcome to the repository!"  
          _//The below setting sets environment variables_  
          env:  
            _//A token that allows the GitHub CLI to authenticate_  
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}  
            _//The URL of the pull request that triggered the event._  
            PR_URL: ${{ github.event.pull_request.html_url }}  
