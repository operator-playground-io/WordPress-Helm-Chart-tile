---
title: Uninstall WordPress helm chart.
description: Learn how to uninstall WordPress helm chart and clean up associated resources.
---

### Uninstall WordPress Helm Chart and Release its Associated Resources

Step 1: To uninstall the wordpress helm chart, use the helm delete command:

```execute
helm delete wordpress -n wordpress
```

You should see the output as below:

```
release "wordpress" uninstalled
```

Executing the command in above step removes all the Kubernetes components associated with the chart and deletes the WordPress release.

Step 2: Check the status if the WordPress chart release still exists.

```execute
helm status wordpress -n wordpress
```

The output shows up as below:

```
Error: release: not found
```

This depicts that the helm chart release and all the resources have been deleted successfully.


### Conclusion

Helm Chart release and all the resources have been deleted successfully.
