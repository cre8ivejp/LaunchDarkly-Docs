---
title: "Git code references"
excerpt: ""
---
[block:api-header]
{
  "title": "Overview"
}
[/block]
This topic explains how to use Git code references in LaunchDarkly to find and manage references to your feature flags.

The Git code references feature lets you find source code references to your feature flags within LaunchDarkly. This makes it easy to determine which projects reference your feature flags, and simplifies removal of technical debt.

We've open-sourced a utility called [`ld-find-code-refs`](https://github.com/launchdarkly/ld-find-code-refs/) that scans your Git repositories and pushes code reference information to LaunchDarkly. You can integrate this utility into your CI / CD process, or take advantage of other trigger mechanisms like [GitHub Actions](doc:github-actions),  `cron` jobs, or Lambda functions triggered by a commit webhook.
[block:api-header]
{
  "title": "Integrating ld-find-code-refs with your toolchain"
}
[/block]
We've designed this feature so that LaunchDarkly does not need any direct access to your source code. The `ld-find-code-refs` utility is agnostic to which git hosting provider you use. 

ur approach makes it possible to push references to LaunchDarkly whether you're using GitHub, GitHub Enterprise, Bitbucket, Bitbucket Enterprise, GitLab, Azure DevOps, or any other Git code hosting tool. 

We support most common trigger mechanisms and CI/CD providers, including:
* [GitHub Actions](doc:github-actions) 
* [Bitbucket Pipelines](doc:bitbucket-pipelines) 
* [CircleCI Orbs](doc:circleci-orbs) 
* [GitLab CI](doc:gitlab-ci) 
* [Custom configuration with the CLI](doc:custom-configuration-via-cli) 

You can also invoke the [`ld-find-code-refs`](https://github.com/launchdarkly/ld-find-code-refs/) utility from the command line. Run this utility in any custom workflow, such as a bash script or cron job.
[block:api-header]
{
  "title": "Prerequisites"
}
[/block]
To set up Git code references in LaunchDarkly, you must have:

* A personal API access token with writer-level access or higher. Create a Personal API access token in the [Access Tokens](https://app.launchdarkly.com/settings/tokens) page. To learn more, read [Personal API access tokens](doc:api-access-tokens).
* You must allow [`ld-find-code-refs`](https://github.com/launchdarkly/ld-find-code-refs/) to run whenever you commit to your git repository. 
[block:callout]
{
  "type": "info",
  "title": "Personal API access tokens can also use custom roles",
  "body": "Alternatively, you can give the access token access to a custom role with the `code-reference-repository` resource specifier. \n\nTo learn more, read [Custom roles](doc:custom-roles)."
}
[/block]

[block:api-header]
{
  "title": "Using code references"
}
[/block]
You can view existing code references for feature flags in the LaunchDarkly dashboard.

To do so, visit a feature flag's **Code references** tab.

The screenshot below shows the Code references tab:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/42f321b-Screen_Shot_2019-01-09_at_2.07.57_PM.png",
        "Screen Shot 2019-01-09 at 2.07.57 PM.png",
        1787,
        859,
        "#f7f7f4"
      ],
      "caption": "The Code references tab for a feature flag."
    }
  ]
}
[/block]
You can also view and edit your connected repositories from the [Integrations](https://app.launchdarkly.com/integrations) page.

The screenshot below shows the Integrations page: 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/c6b1f26-Screen_Shot_2019-01-09_at_11.25.02_AM.png",
        "Screen Shot 2019-01-09 at 11.25.02 AM.png",
        1309,
        285,
        "#f6f7f5"
      ],
      "caption": "The code references repositories section of the Integrations page."
    }
  ]
}
[/block]
You can toggle a repository on or off to allow or forbid code reference triggers from pushing new data to it.
[block:api-header]
{
  "title": "Managing code references"
}
[/block]
Manage code references on the [Integrations](https://app.launchdarkly.com/integrations) page for your project. Code references are organized into repositories, which the `ld-find-code-refs` tool creates automatically. 

After a repository appears on the Integrations page, you can either temporarily disable it (preventing new code references from being added to LaunchDarkly), or delete all code references associated with the repository.

To disable the repository, click the toggle switch to **Off**.
To delete the repository, click **Delete**.

If you click **Delete,** LaunchDarkly purges all data associated with that repository. It will no longer have any record of the code reference or any source context lines. Deleting is permanent and cannot be undone.
[block:callout]
{
  "type": "warning",
  "body": "If you want to remove a connection permanently, be sure to remove any `ld-find-code-refs` triggers from your code. If you're not sure how or where the trigger is invoked, you can also delete the personal access token your trigger uses.\n\nIf you delete a repository with automated code reference updates enabled, the connection is recreated the next time an automated code reference trigger executes.",
  "title": "Remove code references before you click Delete"
}
[/block]
The disabling and delete options appear below:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/185dc51-integrations-delete-repository.png",
        "integrations-delete-repository.png",
        3200,
        814,
        "#e3dedd"
      ],
      "caption": "The code reference repositories section of the Integrations page."
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Configuring context lines"
}
[/block]
The [`ld-find-code-refs`](https://github.com/launchdarkly/ld-find-code-refs/) utility sends two lines of surrounding source context to LaunchDarkly. 

Two lines of code appear above, and two lines appear below the actual reference.

The screenshot below shows code references with context lines:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9de4ef5-Screen_Shot_2019-01-09_at_2.07.57_PM.png",
        "Screen Shot 2019-01-09 at 2.07.57 PM.png",
        1787,
        859,
        "#f7f7f4"
      ],
      "caption": "Code references with context lines."
    }
  ]
}
[/block]
Having a few lines of context can make it easier to understand references to a feature flag. However, these lines are optional. You can disable this feature when you configure `ld-find-code-refs`. 

If context lines are disabled, your code references only show a code location in the form of a filename and line, as well as a link to that location in your Git hosting provider. 

The following screenshot shows code references without context lines:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4176e1a-Screen_Shot_2019-01-09_at_1.28.29_PM.png",
        "Screen Shot 2019-01-09 at 1.28.29 PM.png",
        1845,
        684,
        "#f7f7f7"
      ],
      "caption": "Code references with no context lines."
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Limitations"
}
[/block]
The LaunchDarkly code references API asserts various limits to limit the number of "false positive" code references. `ld-find-code-refs` logs warning messages if you exceed any of these limits. Above certain values, LaunchDarkly may ignore some files and references. 

These limits include:
- We do not scan code references for flags with keys that have less than 3 characters.
- We scan up to 5000 files with code references per repository. LaunchDarkly ignores additional files.
- We allow up to 500 characters is allowed per line of source code stored. LaunchDarkly truncates additional files.
- We allow up to 5000 code references per repository. LaunchDarkly ignores additional references.
- We allow up to 1000 code references per file. LaunchDarkly ignores additional references.

If you've encountered any of these limits, or are noticing a large number of false positives being detected by `ld-find-code-refs`, we'd recommend configuring an `.ldignore` file in your repository with rules matching the files and directories you'd like to exclude.

To learn more about `.ldignore` files, read the [`ld-find-code-refs` README](https://github.com/launchdarkly/ld-find-code-refs#ignoring-files-and-directories).
[block:api-header]
{
  "title": "Using ld-find-code-refs with the React SDK"
}
[/block]
The code references feature scans your source code for occurrences of your flag keys. This process requires your source code to reference flag keys exactly as they appear in LaunchDarkly.

However, by default, the React SDK camel-cases all flag keys for easier access with dot notation. To use the code references feature in conjunction with the React SDK, configure the SDK to disable this camel-casing feature. 

To learn more, read [Flag keys](doc:react-sdk-reference#section-flag-keys) in the React SDK documentation.