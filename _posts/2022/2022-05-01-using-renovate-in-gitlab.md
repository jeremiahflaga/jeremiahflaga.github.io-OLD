---
layout: post
title: Using Renovate in Gitlab
subtitle:
categories: [Programming]
tags: [Programming]
date: 2022-05-01 12:00:00 AM UTC
---

This is a step-by-step guide to auto-update dependencies of GitLab projects using Renovate.

{: .mt-5}

### 0. Create a new Gitlab project using the NodeJS template

![GitLab: create new project using NodeJS template](/images/2022/2022-01-26-gitlab-create-project-using-nodejs-template.png)

We will be using this project in the end part of this tutorial.

{: .mt-5}

### 1. Create a Gitlab project for Renovate (named `renovate-bot` in this tutorial)

![GitLab: create blank project](/images/2022/2022-01-26-gitlab-create-blank-project.png)

{: .mt-5}

### 2. Create a [Personal Access Token (PAT) in GitHub](https://github.com/settings/tokens), with `repo` scope

![GitHub: create new personal access token](/images/2022/2022-01-26-github-new-personal-access-token.png)

{: .mt-5}

### 3. Create a [Personal Access Token (PAT) in GitLab](https://github.com/settings/tokens/new), with `api` scope

![GitLab: create new personal access token](/images/2022/2022-01-26-gitlab-new-personal-access-token.png)

{: .mt-5}

### 4. Add the PAT to the environment variables of your `renovate-bot` GitLab project

Go to `renovate-bot`'s CI/CD Settings

![GitLab: Settings -> CI/CD](/images/2022/2022-01-26-gitlab-settings-ci-cd.png)

Scroll down to the Variables section, then expand it.

![GitLab: Settings -> CI/CD](/images/2022/2022-01-26-gitlab-settings-ci-cd-environment-variables.png)

Then add a variable named `GITHUB_COM_TOKEN` and give it the value of the PAT from GitHub you created in **step 2**.

![GitLab: add `GITHUB_COM_TOKEN` environment variable](/images/2022/2022-01-26-gitlab-add-environment-variable-GITHUB_COM_TOKEN.png)

Add another variable named `RENOVATE_TOKEN` and give it the value of the PAT from GitHub you created in **step 3**.

![GitLab: add `RENOVATE_TOKEN` environment variable](/images/2022/2022-01-26-gitlab-add-environment-variable-RENOVATE_TOKEN.png)

You should have two environment variables added by now, like in this image below:

![GitLab: two environment variables added](/images/2022/2022-01-26-gitlab-two-environment-variables-added.png)

{: .mt-5}

### 5. Go to Repository -> Files page, then click on the "Web IDE" button to open the GitLab IDE

![GitLab: Web IDE button](/images/2022/2022-01-26-gitlab-web-IDE-button.png)

{: .mt-5}

### 6. Create a file named `.gitlab-ci.yml` in your GitLab repository

![GitLab: Web IDE - Create new file button](/images/2022/2022-01-26-gitlab-web-IDE-new-file-button.png)

Paste this into that file:

```yaml
update_repositories:
  image: renovate/renovate:31.55
  script:
    - docker-entrypoint.sh
  variables:
    GITHUB_COM_TOKEN: $GITHUB_COM_TOKEN
    RENOVATE_TOKEN: $RENOVATE_TOKEN
  only:
    - schedules
```

{: .mt-5}

### 7. Create another file named `config.js`,

```js
module.exports = {
  onboardingConfig: {
    extends: ["config:base"],
  },
  platform: "gitlab",
  gitAuthor: "RenovateBot <renovatebot@gmail.com>",
  baseBranches: ["main", "master"],
  labels: ["dependencies"],
  repositories: [
    "jeremiahflaga/renovate-bot",
    "jeremiahflaga/sample-nodejs-app",
  ],
};
```

The `repositories` in that setting contains the names of the projects we created in **step 0** and **step 1** of this tutorial, because those are the projects whose dependencies we want to be updated by Renovate.

**Note** that in your own project you have to change the `repositories` part of that settings.

Please refer to the following tutorial for information about these settings: ["Use Renovate to Manage Dependencies in Gitlab" by Joonas Venäläine](https://faun.pub/use-renovate-to-manage-dependencies-in-gitlab-37fab2b7e847)

{: .mt-5}

### 8. Create another file named `renovate.json`,

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["config:base"]
}
```

Please refer to ["Use Renovate to Manage Dependencies in Gitlab" by Joonas Venäläine](https://faun.pub/use-renovate-to-manage-dependencies-in-gitlab-37fab2b7e847) for an explanation of these settings.

{: .mt-5}

### 9. Commit your changes

![GitLab: Web IDE - commit changes](/images/2022/2022-01-26-gitlab-web-IDE-commit-changes.png)

{: .mt-5}

### 10. Create a schedule to trigger the CI/CD pipeline

Go to CI/CD -> Schedules, then create a new schedule.

![GitLab: create schedule](/images/2022/2022-01-26-gitlab-create-schedule.png)

(Note: you can also manually trigger the schedule.)

{: .mt-5}

### 11. Almost done!!!

When the CI/CD pipeline is triggered, a new merge request will be sent to the `repositories` listed in our `config.js`.

Here is the merge request created for `jeremiahflaga/renovate-bot`:

![GitLab: MR created for `jeremiahflaga/renovate-bot`](/images/2022/2022-01-26-gitlab-merge-request-generated-by-renovate-bot.png)

Here is the merge request created for `jeremiahflaga/sample-nodejs-app`:

![GitLab: MR created for `jeremiahflaga/sample-nodejs-app`](/images/2022/2022-01-26-gitlab-merge-request-generated-by-renovate-bot-2.png)

{: .mt-5}

### 12. Merge those merge requests... and we're done :smiley:

**Note:** If you want the merge request to be directed to other branches, like `development` or `production`, you have to overwrite the `renovate.json` file in those target repositories like this:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["config:base"],
  "dependencyDashboard": true,
  "baseBranches": ["development", "production"]
}
```

(Note: update the `renovate.json` in the `main` or `master` branch.)

Please refer to ["Use Renovate to Manage Dependencies in Gitlab" by Joonas Venäläine](https://faun.pub/use-renovate-to-manage-dependencies-in-gitlab-37fab2b7e847) for an explanation of these settings.

---

In the next minutes/hours/days, depending on the schedule of the CI/CD pipeline, the Renovate bot will create new merge requests like this:

![GitLab: Other MR created by the Renovate bot](/images/2022/2022-01-26-gitlab-merge-request-generated-by-renovate-bot-3.png)

---

<div class="small" markdown="1">

### References

**Main reference:** ["Use Renovate to Manage Dependencies in Gitlab"](https://faun.pub/use-renovate-to-manage-dependencies-in-gitlab-37fab2b7e847) by Joonas Venäläinen (January 19, 2022)

**Other references:**

["Renovate Your GitLab Projects Automatically"](https://dev.to/openpatch/patcher-renovate-your-gitlab-projects-automatically-1p6e) by Mike Barkmin

["How to Keep Software Dependencies Up-to-Date with Renovate"](https://codeburst.io/how-to-keep-software-dependencies-up-to-date-with-renovate-2f274fe8da47) by Emmanuel Sys

["Update dependencies with Renovate"](https://dev.to/arxeiss/update-dependencies-with-renovate-37on) by Pavel Kutáč

["Automatic Dependency Updates with Renovate and Gitlab"](https://fotoallerlei.com/blog/post/2020/automatic-dependency-updates-with-renovate-and-gitlab/post) by Max Rosin

["How to Update Dependencies Safely and Automatically with GitHub Actions and Renovate"](https://www.freecodecamp.org/news/update-dependencies-automatically-with-github-actions-and-renovate/) by Ramón Morcillo

</div>
