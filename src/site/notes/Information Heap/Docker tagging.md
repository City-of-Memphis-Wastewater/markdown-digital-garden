---
{"dg-publish":true,"permalink":"/information-heap/docker-tagging/","noteIcon":"","created":"2025-10-28T10:16:38.045-05:00"}
---

Date: [[-Daily Activity Log-/2025 10-October 28\|2025 10-October 28]]

# *Ensuring* Version Tags on GHCR Packages

When using a Container Registry like `ghcr.io`, it is best practice to always push your image with a meaningful tag (e.g., `v1.0.0`, `latest`, or a short commit SHA) rather than relying on the anonymous digest.

The format for pushing a tagged image to your registry is: `ghcr.io/ORG_OR_USER/REPO_NAME/IMAGE_NAME:TAG`

Your current package seems to be addressed as: `ghcr.io/City-of-Memphis-Wastewater/pipeline/pipeline-eds`

## 1. Get a Meaningful Tag

If you are using a CI/CD system (like GitHub Actions), you can use environment variables or Git commands to generate a tag.

|Tag Type|Command (in Linux/Bash)|Example Result|Use Case|
|---|---|---|---|
|**Commit SHA**|`git rev-parse --short HEAD`|`a1b2c3d`|For every commit/build, especially dev/staging environments.|
|**Git Tag (Version)**|`git describe --tags --always`|`v0.2.113`|For release builds, tying the container to a specific release tag.|
|**Date/Time**|`date +%Y%m%d%H%M%S`|`20251028101430`|For chronological versioning when no Git tags are used.|

## 2. The Build and Tag Process

Replace the generic `IMAGE_NAME` with `pipeline-eds` and `ORG_OR_USER` with `City-of-Memphis-Wastewater`.

1. **Define the Tag:** Create a variable for your desired tag. For example, using the latest Git tag:
    
    ```
    IMAGE_TAG=$(git describe --tags --always)
    ```
    
2. **Build the Image:** Build the image locally, giving it a name for the next step.
    
    ```
    docker build -t pipeline-eds .
    ```
    
3. **Tag the Image:** Apply the full registry path and the version tag to the local image.
    
    ```
    docker tag pipeline-eds ghcr.io/City-of-Memphis-Wastewater/pipeline/pipeline-eds:$IMAGE_TAG
    ```
    
4. **Push the Image:** Push the fully tagged image.
    
    ```
    docker push ghcr.io/City-of-Memphis-Wastewater/pipeline/pipeline-eds:$IMAGE_TAG
    ```
    

### Recommended Workflow Adaptation

To also maintain the traditional `latest` tag, you should repeat the tagging and pushing for the `latest` tag in the same script:

```
# ... (Steps 1, 2, and 3 above for the version tag)

# 4. Push the Version Tag
docker push ghcr.io/City-of-Memphis-Wastewater/pipeline/pipeline-eds:$IMAGE_TAG

# 5. Tag for 'latest' (optional, but recommended)
docker tag pipeline-eds ghcr.io/City-of-Memphis-Wastewater/pipeline/pipeline-eds:latest

# 6. Push the 'latest' Tag
docker push ghcr.io/City-of-Memphis-Wastewater/pipeline/pipeline-eds:latest
```