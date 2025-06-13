# Prompts to Generate GEI Migration GitHub Actions Workflow

1. Generate a GitHub workflow that migrates a JSON array of GitHub repository names for an organization in YAML.
1. Write another one using the GitHub Enterprise Importer tool.
1. Add the ability to migrate more than one repo at a time.
1. Change the input to read from a file in the repository.
1. Add a variable that sets the number of parallel migrations that can be run.
1. Change the repos.json path to a file path and the organization name to an input variable.
1. Make the source org an input as well.
1. Add notifications for Slack and Microsoft Teams, a dry-run mode, and post migration success/failure reporting.
1. Remove the command `gh audit-repo` and make it an api call using the current source token and org in the form of `gh api /repos` 
1. Add the a summary notification + Slack threading support.
1. Write documentation for this workflow in markdown format.
1. Add a .gitignore.
1. Add GitHub labels.
1. Yes, and explain the labels.
1. Add a short paragraph at the start of the documentation explaining prompt engineering and add a file with the prompts that generated this code using ChatGPT
