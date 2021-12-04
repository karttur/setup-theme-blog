---
layout: post
title: GitHub personal token
previousurl: null
nexturl: null
excerpt: "How to use a personal token for accessing GitHub using the command line"
date: "2021-12-03"
modified: "2021-12-03"
categories: "blog"
tags:
  - GitHub
  - personal token
  - CLI
image: rntwi-delta_RNTWI_java_2001-2016_mk-z-ts-model
comments: true
share: true
---

## Introduction

In August 2021 [GitHub](https://github.com) changed the access security. Login with user and password are not longer allowed. Instead of password, GitHub expects a [personal access token](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/). This post summarizes how to implement command line access to GitHub with a personal token. On MacOS.

The solution I put in place is based on a [GitHub Command Line Interface (CLI) Credential manager (GCM)](https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git). Here are the steps to make it work:

1. Open the <span class='app'>Terminal</span>

2. Install the GCM with <span class='termnalapp'>brew</span>:

<span class='terminal'>$ brew install gh</span>

3. Run the command

<span class='terminal'>$ gh auth login </span>

You will be prompted to answer a series of questions:

```
% gh auth login
? What account do you want to log into? GitHub.com
? What is your preferred protocol for Git operations? HTTPS
? Authenticate Git with your GitHub credentials? Yes
? How would you like to authenticate GitHub CLI? Paste an authentication token
Tip: you can generate a Personal Access Token here https://github.com/settings/tokens
The minimum required scopes are 'repo', 'read:org', 'workflow'.
? Paste your authentication token: ****************************************
```

To generate the Personal Access Token I used the link [https://github.com/settings/tokens](https://github.com/settings/tokens). The token is only shown once and you have to copy it directly, then paste it at the command line.
