<!--
title: Deploy Serverless Hexo Website
description: "Build a serverless hexo website with serverless website component"
date: 2019-11-28
thumbnail: 'http://url-to-thumbnail.jpg'
categories:
  - toturial
authors:
  - Tinafang
authorslink:
  - https://github.com/tinafangkunding
translators:
  - None
translatorslink:
  - None
-->

# Build a Serverless Hexo Website

## Quick Start

Build a serverless hexo website with serverless website component, it's fast and no need to pay attention on configuring different cloud services.

&nbsp;

- [请点击这里查看中文版部署文档](./README.md)

&nbsp;

1. [Install](#1-install)
2. [Configure](#2-configure)
3. [Deploy](#3-deploy)
4. [Remove](#4-remove)
5. [Account (optional)](#5-account-optional)

&nbsp;

### 1. Install

**Applications you need to install before deploying**

- [Node.js](https://nodejs.org/en/) (Should be at least Node.js 8.6, recommends 10.0 or higher)
- [Git](https://git-scm.com/)

If you don't install these applications, you could go to [Installation](https://hexo.io/docs/index.html) as a reference.

**Install Serverless Framework**

```
$ npm install -g serverless
```

**Install Hexo**

```
$ npm install -g hexo-cli
```

Once Hexo is installed, run the following commands to initialize Hexo in the target `hexo` folder.

```
$ hexo init hexo   # init a hexo folder
$ cd hexo
$ npm install
```

Once initialized, here’s what your project folder will look like:

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

After installation, you could use `hexo g` to generate static pages.

```
hexo g   # generate
```

> Note: You could use the following command and visit `localhost:4000` to test the hexo website locally.

```
hexo s   # server
```

### 2. Configure

Create `serverless.yml` file in the project, and configure the yaml.

```console
$ touch serverless.yml
```

```yml
# serverless.yml

myWebsite:
  component: '@serverless/tencent-website'
  inputs:
    code:
      src: ./hexo/public # Upload static files generated by HEXO
      index: index.html
      error: index.html
    region: ap-guangzhou
    bucketName: my-bucket
```

Here’s what your project folder will look like after configuration:

```
.
├── .serverless
├── hexo
|   ├── public
|   ├── ...
|   ├── _config.yml
|   ├── ...
|   └── source
└── serverless.yml
```

### 3. Deploy

Use `sls` command to deploy your project, you could also add `--debug` to see the detail information in the process.

If you have a `Wechat` account, you don't need to configure the `.env` file, just scan the QR code in terminal and sign-up a new account of Tencent Cloud. It's a streamlined experience.

If you don't have a wechat account, you could jump to [account](#5-account-optional) step and configure the account info.

```
$ serverless --debug

  DEBUG ─ Resolving the template's static variables.
  DEBUG ─ Collecting components from the template.
  DEBUG ─ Downloading any NPM components found in the template.
  DEBUG ─ Analyzing the template's components dependencies.
  DEBUG ─ Creating the template's components graph.
  DEBUG ─ Syncing template state.
  DEBUG ─ Executing the template's components graph.
  DEBUG ─ Starting Website Component.

Please scan QR code login from wechat
Wait login...
Login successful for TencentCloud
  DEBUG ─ Preparing website Tencent COS bucket my-bucket-1250000000.
  DEBUG ─ Deploying "my-bucket-1250000000" bucket in the "ap-guangzhou" region.
  DEBUG ─ "my-bucket-1250000000" bucket was successfully deployed to the "ap-guangzhou" region.
  DEBUG ─ Setting ACL for "my-bucket-1250000000" bucket in the "ap-guangzhou" region.
  DEBUG ─ Ensuring no CORS are set for "my-bucket-1250000000" bucket in the "ap-guangzhou" region.
  DEBUG ─ Ensuring no Tags are set for "my-bucket-1250000000" bucket in the "ap-guangzhou" region.
  DEBUG ─ Configuring bucket my-bucket-1250000000 for website hosting.
  DEBUG ─ Uploading website files from D:\hexotina\localhexo\public to bucket my-bucket-1250000000.
  DEBUG ─ Starting upload to bucket my-bucket-1250000000 in region ap-guangzhou
  DEBUG ─ Uploading directory D:\hexotina\localhexo\public to bucket my-bucket-1250000000
  DEBUG ─ Website deployed successfully to URL: https://my-bucket-1250000000.cos-website.ap-guangzhou.myqcloud.com.

  myWebsite:
    url: https://my-bucket-1250000000.cos-website.ap-guangzhou.myqcloud.com
    env:

  13s » myWebsite » done
```

Visit the website url in terminal's output and your hexo website is finished!

> Note: If you want to update posts in hexo, you need to run `hexo g` to generate the pages and then re-deploy it via `serverless` command.

### 4. Remove

Use the following command to remove the project

```console
$ sls remove --debug

  DEBUG ─ Flushing template state and removing all components.
  DEBUG ─ Starting Website Removal.
  DEBUG ─ Removing Website bucket.
  DEBUG ─ Removing files from the "my-bucket-1250000000" bucket.
  DEBUG ─ Removing "my-bucket-1250000000" bucket from the "ap-guangzhou" region.
  DEBUG ─ "my-bucket-1250000000" bucket was successfully removed from the "ap-guangzhou" region.
  DEBUG ─ Finished Website Removal.

  6s » myWebsite » done

```

### 5. Account (optional)

Just create a `.env` file

```console
$ touch .env # your Tencent API Keys
```

Add the access keys of a [Tencent CAM Role](https://console.cloud.tencent.com/cam/capi) with `AdministratorAccess` in the `.env` file, using this format:

```
# .env
TENCENT_SECRET_ID=123
TENCENT_SECRET_KEY=123
```

- If you don't have a Tencent Cloud account, you could [sign up](https://intl.cloud.tencent.com/register) first.
