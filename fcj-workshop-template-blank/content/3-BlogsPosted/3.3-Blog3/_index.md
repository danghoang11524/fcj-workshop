---
title: "Blog 3"
date: 2026-07-21
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Real-World Operations: Automated CI/CD and Data Protection with PITR

This is the third blog in my **Smart Notes API** series. While the first two articles focused on building and securing the system, this one covers a topic that often receives less attention but is just as important: **operations**.

## The Problem

Once the API was running smoothly and properly secured, I identified two major issues:

1. Every code change required manually running `sam build && sam deploy` on my local machine. It was easy to forget running tests, and only my computer could perform deployments.
2. Accidentally deleting data—whether caused by human error or a software bug—would permanently remove it. DynamoDB does not provide a built-in "table recycle bin."

The solution was to implement **CI/CD** and **Point-in-Time Recovery (PITR)**.

## CI/CD with GitHub Actions

AWS doesn't provide a single all-in-one CI/CD service—you assemble the workflow using different tools. For a personal project, I chose **GitHub Actions** instead of AWS CodePipeline and CodeBuild because it is simpler and doesn't introduce additional AWS costs.

Whenever code is pushed to the `main` branch, the workflow automatically performs:

```
Checkout code → Install dependencies → Run unit tests (Jest)
→ If successful → sam build → sam deploy
```

The most important part is that **all tests run before deployment**.

If any test fails, the pipeline stops immediately, and the production infrastructure remains untouched. This makes deployments much safer and gives me greater confidence when pushing new code.

One detail that took some time to figure out was handling the Google Client ID and Client Secret used by Amazon Cognito. Since both parameters are marked as `NoEcho`, they are not stored in `samconfig.toml`. Instead, the workflow passes them through `--parameter-overrides`, with the values securely stored in GitHub Secrets rather than hardcoded in the repository.

## Point-in-Time Recovery (PITR) for DynamoDB

This is probably the feature that delivered the greatest benefit for the least amount of effort—just two lines added to `template.yaml`:

```yaml
NotesTable:
  Properties:
    PointInTimeRecoverySpecification:
      PointInTimeRecoveryEnabled: true
```

Once enabled, DynamoDB continuously backs up the table and allows it to be restored to **any point in time within the previous 35 days**.

There is no need to write backup scripts or schedule EventBridge jobs. For a personal application, the additional cost is minimal while providing a valuable safety net.

A sample restore command looks like this:

```bash
aws dynamodb restore-table-to-point-in-time \
  --source-table-name Notes-dev \
  --target-table-name Notes-dev-restored \
  --restore-date-time "2026-07-20T10:00:00Z"
```

One important thing to remember is that PITR restores data into a **new table**. It never overwrites the existing production table, so migrating the restored data back into production requires additional manual steps.

## Looking Back

The biggest takeaways from this project were:

- CI/CD with GitHub Actions ensures unit tests always run before deployment, preventing broken code from reaching production.
- Sensitive parameters marked as `NoEcho` should be passed through `--parameter-overrides` using GitHub Secrets instead of being hardcoded.
- DynamoDB Point-in-Time Recovery requires only two lines of configuration while enabling restoration to any point within the previous 35 days.
- PITR creates a new table during recovery rather than replacing the existing one, so restoring production data requires an additional migration step.

The biggest lesson from my internship is that there is a significant difference between a system that **works** and a system that can be **operated reliably in the long term**. AWS already provides most of the tools needed—you simply need to identify what's missing and choose the right services to solve the problem.

**Read More:**

- **Blog 1:** Building Smart Notes API
- **Blog 2:** Securing Smart Notes API with API Keys and Amazon Cognito
- **Original post on the AWS Study Group FCJ Facebook community**