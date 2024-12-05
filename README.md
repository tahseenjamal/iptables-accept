# Setting Up iptables INPUT ACCEPT on Startup for Run-level 5

This guide will help you create a script that automatically sets the default policy for the `iptables` INPUT chain to `ACCEPT` after every reboot, specifically when entering **run-level 5**.

### Step 1: Create the Init Script

1. Open a new file named `iptables-accept` in the `/etc/init.d/` directory:
   ```bash
   sudo vi /etc/init.d/iptables-accept
   ```

2. Add the following content to the script:
   ```sh
   #!/bin/sh
   ### BEGIN INIT INFO
   # Provides:          iptables-accept
   # Required-Start:    $network
   # Required-Stop:
   # Default-Start:     5
   # Default-Stop:
   # Short-Description: Set iptables INPUT policy to ACCEPT
   ### END INIT INFO

   /sbin/iptables -P INPUT ACCEPT
   ```

### Step 2: Make the Script Executable

To ensure the script can be executed, run the following command:
```bash
sudo chmod +x /etc/init.d/iptables-accept
```

### Step 3: Create a Symbolic Link in `rc5.d`

To make sure the script runs during system startup in run-level 5, create a symbolic link in the `rc5.d` directory:
```bash
sudo ln -s /etc/init.d/iptables-accept /etc/rc5.d/S99iptables-accept
```
The link name `S99iptables-accept` ensures that the script runs late in the sequence, after most other services have started.

### Verification

1. **Check the Link**: Verify that the symbolic link has been created successfully by listing the contents of `/etc/rc5.d/`:
   ```bash
   ls -l /etc/rc5.d/
   ```
   You should see an entry like:
   ```
   S99iptables-accept -> /etc/init.d/iptables-accept
   ```

2. **Test the Script**: You can also test the script manually by running:
   ```bash
   sudo /etc/init.d/iptables-accept
   ```
   This should set the default `iptables` policy for the `INPUT` chain to `ACCEPT`.

### Summary

By following these steps, you have created a script that automatically sets the default policy of the `iptables` `INPUT` chain to `ACCEPT` every time your server boots into run-level 5. This ensures that incoming connections are allowed by default after each reboot, keeping your server accessible.

