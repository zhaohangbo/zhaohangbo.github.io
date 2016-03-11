---
layout: post
title: How to Contribute to Openstack
category: 技术
tags: [Openstack]
keywords: Openstack
---

### Developer’s Guide

[Developer’s Guide](http://docs.openstack.org/infra/manual/developers.html)

### Learn the Gerrit Workflow in the Sandbox

[Learn the Gerrit Workflow in the Sandbox](http://docs.openstack.org/infra/manual/sandbox.html#sandbox)


If you're getting a permission denied, check 2 things.
1. Double-check that the username at https://review.openstack.org/#/settings/ is right.
2. Check if your SSH key at https://review.openstack.org/#/settings/ssh-keys matches the contents of your ~/.ssh/id_rsa.pub file.

```
Could not connect to gerrit.
Enter your gerrit username: zhaohangbo
Trying again with ssh://zhaohangbo@review.openstack.org:29418/openstack-dev/sandbox.git
<traceback object at 0x1101c37e8>
We don't know where your gerrit is. Please manually create a remote
named "gerrit" and try again.
Could not connect to gerrit at ssh://@review.openstack.org:29418/openstack-dev/sandbox.git
```

git remote add gerrit ssh://username@review.openstack.org:29418/openstack-xxx/project.git


### MacVim to work with Git Commit

git config --global core.editor '/usr/local/bin/mvim -f --nomru -c "au VimLeave * !open -a iTerm "'
