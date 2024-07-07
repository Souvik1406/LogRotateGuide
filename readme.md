# Step-by-Step Guide for Log Rotation

## Step 1: Install Required Tools

Ensure the `logrotate` tool is installed on your system. If it's not already installed, you can install it using the following commands:

```bash
sudo apt-get update
sudo apt-get install logrotate
```

## Step 2: Configure Logrotate

1. **Create/Edit Logrotate Configuration File**

   Create a configuration file specific to the log you want to rotate. For example, create a file named `myapplog` in the `/etc/logrotate.d/` directory:

   ```bash
   sudo nano /etc/logrotate.d/myapplog
   ```

2. **Add Configuration Settings**

   Add the following configuration settings to rotate logs every hour, compress them, and keep up to 5 compressed log files:

   ```text
   /path/to/your/logfile.log {
       hourly
       rotate 5
       compress
       delaycompress
       missingok
       notifempty
       create 0640 root root
       olddir /path/to/your/old/logs
   }
   ```

   Explanation of the settings:

   - `/path/to/your/logfile.log`: Path to your log file.
   - `hourly`: Rotate logs every hour.
   - `rotate 5`: Keep 5 compressed log files before deleting the oldest.
   - `compress`: Compress the rotated log files.
   - `delaycompress`: Postpone compression of the previous log file until the next rotation cycle.
   - `missingok`: Do not issue an error message if the log file is missing.
   - `notifempty`: Do not rotate the log if it is empty.
   - `create 0640 root root`: Recreate the log file after rotation with specified permissions and ownership.
   - `olddir /path/to/your/old/logs`: Move old log files to the specified directory.

   Ensure you replace `/path/to/your/logfile.log` and `/path/to/your/old/logs` with the actual paths on your server.

### Step 3: Test Logrotate Configuration

1. **Check Configuration Syntax**

   Before deploying, verify that your configuration file syntax is correct:

   ```bash
   sudo logrotate -d /etc/logrotate.d/myapplog
   ```

   This command will simulate the log rotation process and display debug output.

2. **Manually Trigger Rotation**

   Test the log rotation manually to ensure it works as expected:

   ```bash
   sudo logrotate -f /etc/logrotate.d/myapplog
   ```

### Step 4: Automate Log Rotation Using Cron

1. **Edit Crontab**

   To automate log rotation every hour, add a cron job. Open the crontab editor:

   ```bash
   sudo crontab -e
   ```

2. **Add Cron Job**

   Add the following line to run logrotate hourly:

   ```bash
   0 * * * * /usr/sbin/logrotate /etc/logrotate.d/myapplog --state /var/lib/logrotate/status
   ```

   This cron job will run `logrotate` every hour at minute 0 (`0 * * * *`).

### Step 5: Monitor and Maintain

1. **Monitor Logs and Rotation**

   Regularly check the rotated logs to ensure they are being managed correctly and that you are not running out of disk space.

2. **Adjust Configuration as Needed**

   Modify the log rotation settings (e.g., `rotate`, `size`, etc.) based on the volume of log data and storage requirements.

## Example Logrotate Configuration File

Here’s an example of what your `/etc/logrotate.d/myapplog` file might look like:

```text
/var/log/myapp.log {
    hourly
    rotate 5
    compress
    delaycompress
    missingok
    notifempty
    create 0640 root root
    olddir /var/log/myapp/old
}
```

## Conclusion

By following these steps, you can set up log rotation on a Linux server to rotate logs every hour, compress them into `.gz` files, and maintain up to 5 compressed log files. This setup helps manage log file sizes and ensures that old log data is systematically archived and rotated out. Regular monitoring and adjustments will help keep your log management efficient and effective.


