# base-alpine-nginx Docker Base Image Specification

#### Overview
This specification outlines the requirements and configurations for a Docker base image used to run NGINX with a custom user and specific configurations on an Alpine Linux environment. The base image is intended for web applications needing a lightweight and secure environment.

#### Base Image
- **Image Name:** `nginx`
- **Image Tag:** `mainline-alpine`
- **Image Digest:** `@${ARCH_DIGEST_SHA}`

#### Environment Variables
- **DEBIAN_FRONTEND:** `noninteractive`

#### Alpine Linux Repositories
- Add the community repository:
  ```
  http://dl-2.alpinelinux.org/alpine/edge/community/
  ```

#### Packages
- **Shadow Package:** Required for managing users and groups.
- **Git:** Required for version control operations.

#### User Configuration
- **Username:** `web_runtime_user`
- **User Details:** 
  - `First Last, RoomNumber, WorkPhone, HomePhone`
- **Password:** 
  - Predefined hashed password: `cdc01cb19cc1ee86b2bc4aa5c67d6f5b9d3866225a5c3b0f5df1866a6f371fe4`
- **User Groups:** 
  - `www-data`: To allow access to web-related directories and files.

#### Timezone
- **Timezone:** `UTC`
  - Symlink `/usr/share/zoneinfo/UTC` to `/etc/localtime`

#### Supervisor Configuration
- **Supervisor Configuration Files:**
  - `supervisor.conf` should be located at:
    - `/etc/supervisor/conf.d/supervisor.conf`
    - `/etc/supervisord.conf`
  - These configurations ensure Supervisor manages processes and keeps the container alive.

#### NGINX Configuration
- **NGINX Main Configuration:**
  - Copy custom `nginx.conf` to `/etc/nginx/nginx.conf`
- **NGINX Additional Configurations:**
  - Copy custom configurations to `/etc/nginx/conf.d`

#### Log Files
- **Log Directory:** `/tmp/log/nginx`
  - Create necessary log files:
    - `app-error.log`
    - `access.log`
- **Permissions:** 
  - Set ownership of `/tmp/log` and `/var/log/nginx` to `web_runtime_user:www-data`

#### Permissions
- **Executable Scripts:** 
  - Make all `.sh` scripts in the root directory executable

#### Cleanup
- **Remove Temporary Files:**
  - Clean up `/tmp` and `/var/cache/apk` directories to reduce image size.
