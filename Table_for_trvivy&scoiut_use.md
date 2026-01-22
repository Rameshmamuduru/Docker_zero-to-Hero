| Stage                        | Tool          | What to do                                           | Why                                                         |
| ---------------------------- | ------------- | ---------------------------------------------------- | ----------------------------------------------------------- |
| **Before build**             | Trivy         | Scan **base image**, scan **source code/filesystem** | Prevent using insecure base image; detect code issues early |
|                              | Docker Scout  | Quick overview of **base image layers and size**     | Identify potential bloat or inefficient base image          |
| **After build (pre-deploy)** | Trivy         | Scan **built image for CVEs**, fail on HIGH/CRITICAL | Ensure the image you deploy is secure                       |
|                              | Docker Scout  | Check **image layers, size, recommendations**        | Optimize image, prevent bloated or inefficient images       |
| **Post-deploy**              | Trivy + Scout | Scheduled scans of **registry images**               | Detect vulnerabilities discovered after deployment          |
