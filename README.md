# W&B ChatOps Assignment

This repository contains a GitHub Actions workflow that implements chatops for Weights & Biases (W&B) model comparison.

## Features

The workflow listens for comments on pull requests in the format `/wandb <run_id>` and:
1. ✅ Fetches the run tagged as "baseline" from your W&B project
2. ✅ Retrieves the specified run by ID
3. ✅ Generates a comparison URL between the baseline and the specified run
4. ✅ Comments on the PR with the comparison report URL

## Setup

### 1. Repository Secrets

Add the following secrets to your GitHub repository (Settings → Secrets and variables → Actions):

- `WANDB_API_KEY`: Your W&B API key (get it from https://wandb.ai/authorize)
- `WANDB_ENTITY`: Your W&B username or team name
- `WANDB_PROJECT`: Your W&B project name

### 2. Tag a Baseline Run

In your W&B project, tag at least one run with the tag `baseline`. This run will be used as the comparison baseline.

You can tag a run using:
- W&B UI: Go to the run page → Tags → Add tag "baseline"
- W&B CLI: `wandb tag baseline <run_id>`
- W&B API: `run.tags.append("baseline")`

### 3. Make Repository Public

The assignment requires your repository to be public so the workflow link can be accessed.

## Usage

1. Open a pull request in this repository
2. Comment on the PR with: `/wandb <run_id>`
   - Example: `/wandb abc123def456`
3. The workflow will automatically:
   - Extract the run ID from your comment
   - Fetch the baseline run
   - Create a comparison URL
   - Comment back on the PR with the comparison report

## Workflow File

The main workflow file is located at:
```
.github/workflows/wandb-chatops.yml
```

## Files

- `.github/workflows/wandb-chatops.yml`: GitHub Actions workflow definition
- `wandb_comparison.py`: Python script that handles W&B API calls
- `requirements.txt`: Python dependencies

## Example

```
User comments: /wandb xyz789abc123

Workflow responds:
## W&B Comparison Report

✅ Successfully compared run `xyz789abc123` with baseline.

📊 [View Comparison Report](https://wandb.ai/your-entity/your-project/compare?run=baseline-run-id&run=xyz789abc123)
```

## Testing

To test the workflow:
1. Ensure all secrets are configured
2. Create a test PR
3. Comment `/wandb <valid-run-id>` on the PR
4. Check the Actions tab to see the workflow execution
5. The PR should receive a comment with the comparison URL
