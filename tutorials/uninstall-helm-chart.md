---
title: How to Uninstall WordPress chart 
description: This tutorial explains how to Uninstall WordPress helm chart
---

### Uninstall WordPress Helm Chart and release the associated resources 

To uninstall the wordpress helm chart, use the helm delete command:

```execute
helm delete wordpress -n wordpress
```

Output:

```
release "wordpress" uninstalled
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

```execute
helm status wordpress -n wordpress
```

Output:

```
Error: release: not found
```
