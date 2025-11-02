---
{"dg-publish":true,"permalink":"/information-heap/build-pipeline-0-2-115-for-docker/","noteIcon":"","created":"2025-10-04T04:25:04.537-05:00"}
---

Date: [[-Daily Activity Log-/2025 11-November 02\|2025 11-November 02]]


```PowerShell

docker buildx build `
    --platform linux/amd64,linux/arm64  `
    -t ghcr.io/city-of-memphis-wastewater/pipeline-eds:multi-dev `
    -f Docker.multi-dev .
```

### in Ubuntu WSL2:
```bash
docker buildx build \
    --platform linux/amd64,linux/arm64 \
	--label org.opencontainers.image.description="A lightweight, multi-architecture command-line tool for runtime and environment introspection." \
    -t ghcr.io/city-of-memphis-wastewater/pyhabitat:1.0.35-cli \
    -t ghcr.io/city-of-memphis-wastewater/pyhabitat:multi-dev \
    -t ghcr.io/city-of-memphis-wastewater/pyhabitat:1.0.35-multi-dev \  
    -f Dockerfile.multi-dev . \
    --push
    
docker buildx build --platform linux/amd64,linux/arm64 -t ghcr.io/city-of-memphis-wastewater/pyhabitat:multi-dev -t ghcr.io/city-of-memphis-wastewater/pyhabitat:1.0.35-multi-dev -f Dockerfile.multi-dev . --push

```
# multiple tags:
```
docker buildx build \
    --platform linux/amd64,linux/arm64 \
    -t ghcr.io/city-of-memphis-wastewater/pyhabitat:1.0.35 \
    -t ghcr.io/city-of-memphis-wastewater/pyhabitat:dev \
    -f Dockerfile . \
    --push

docker buildx build \ 
	--platform linux/amd64,linux/arm64 \ 
	--build-arg PYHABITAT_VERSION=1.0.35 \ 
	-t ghcr.io/city-of-memphis-wastewater/pyhabitat:1.0.35-cli \ 
	-t ghcr.io/city-of-memphis-wastewater/pyhabitat:1.0.35-pyz \
	-t ghcr.io/city-of-memphis-wastewater/pyhabitat:1.0.35 \ 
	-f Dockerfile . \ 
	--push
```

# run
```
docker run -it ghcr.io/city-of-memphis-wastewater/pyhabitat:1.0.35
```
## [0.2.115] - 2025-10-04

This release focuses on significant improvements to the **Windows Installation and Setup** process, providing a more professional, silent, and integrated user experience for standalone executable users.

### Added

- **Silent Installation Logging (Windows):** Implemented file-based logging to `install_log.txt` within the `AppData` configuration directory. All verbose setup output is now redirected from `stdout` to this file, ensuring a non-disruptive, single-line console status message during application launch.
    
- **Start Menu Shortcut:** Automated creation of a launcher `.BAT` file in the user's Start Menu Programs directory (`%APPDATA%\Microsoft\Windows\Start Menu\Programs`), significantly improving application accessibility.
    
- **Comprehensive Uninstall/Cleanup (Windows):** Introduced `cleanup_windows_install()` and supporting routines to reliably remove all generated installation artifacts (Desktop launcher, Start Menu shortcut, Context Menu Registry keys, and AppData installation files) upon request.
    

### Changed

- **Installation Dispatcher Optimization (Windows):** Implemented version-based installation tracking using an `install_version.txt` file in AppData. This prevents all setup routines (registry, shortcuts) from re-running on every application launch, eliminating startup overhead.
    
- **Context Menu Information:** The `setup_info_eds.ps1` PowerShell script, accessible via the folder background right-click, is now dynamically populated to explicitly reference the executable type (`.EXE` or `.PYZ`) and includes the definitive link to the GitHub Releases page for direct download options.
    

### Fixed

- **Excessive Setup Output:** Resolved the issue where verbose installation details were printed to the console during every application startup by implementing the version-based state check and redirecting all setup messages to the log file.