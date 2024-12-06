---
tags:
    - ubuntu
    - server
    - software
    - nextcloud
    - installation
---

# New User Config

To prevent the creation of default files in new user accounts in Nextcloud (such as the **Files** app's "Documents", "Photos", "Music" directories, etc.), you will need to disable or remove the default files that are generated when a new user is created.


### Delete Default Files in the `user-skeleton` Directory

New users in Nextcloud are typically provided with default folders/files from a `user-skeleton` directory.

- Locate the `user-skeleton` directory. The default path is usually:

   ```bash
   cd /var/www/nextcloud/core/skeleton
   ```

- Delete any files or directories in this folder to prevent them from being copied to new users' home directories:

!!! note
    This will prevent any default files (such as `Documents`, `Photos`, etc.) from being copied to new user accounts.

### Prevent "First Run Wizard" (Optional)

The "First Run Wizard" in Nextcloud might also encourage the creation of default folders. If you want to remove this wizard for new users, follow these steps:

**Disable the First Run Wizard**:
   - As an admin, you can disable the "First Run Wizard" by navigating to:
   - **Settings** > **Administration** > **General**.
   - Then, uncheck the option **Enable first-run wizard**.

   Alternatively, you can do this via the `config.php` file:

   ```php
   'show_first_run_wizard' => false,
   ```

<script data-name="BMC-Widget" data-cfasync="false" src="https://cdnjs.buymeacoffee.com/1.0.0/widget.prod.min.js" data-id="justaguylinux" data-description="Support me on Buy me a coffee!" data-message="" data-color="#FF5F5F" data-position="Right" data-x_margin="18" data-y_margin="18"></script>
