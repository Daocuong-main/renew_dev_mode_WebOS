### Step-by-Step Instructions

1. **Create the Shell Script**:
   ```sh
   nano ~/dev_mode.sh
   ```

   Add the following content to the file:
   ```sh
   #!/bin/bash

   URL="fill_url_in_here"
   LOGFILE="/var/log/request.log"

   echo "Starting script..." >> $LOGFILE

   while true; do
       RESPONSE=$(curl -s -o /dev/null -w "%{http_code}" $URL)
       if [ $RESPONSE -eq 200 ]; then
           echo "$(date): Request successful" >> $LOGFILE
       else
           echo "$(date): Request failed with status $RESPONSE" >> $LOGFILE
       fi
       sleep 2d
   done
   ```

2. **Make the Script Executable**:
   ```sh
   chmod +x ~/dev_mode.sh
   ```

3. **Create a Systemd Service**:
   ```sh
   sudo nano /etc/systemd/system/request.service
   ```

   Add the following content to the file:
   ```ini
   [Unit]
   Description=Periodic URL Request Service

   [Service]
   ExecStart=/home/cuong/dev_mode.sh
   Restart=always

   [Install]
   WantedBy=multi-user.target
   ```

4. **Enable and Start the Service**:
   ```sh
   sudo systemctl enable request.service
   sudo systemctl start request.service
   ```

5. **Check the Status of the Service**:
   ```sh
   sudo systemctl status request.service
   ```

6. **Check the Log for Success**:
   ```sh
   sudo tail -f /var/log/request.log
   ```

### Managing the Service

- **Turn On**: `sudo systemctl start request.service`
- **Turn Off**: `sudo systemctl stop request.service`

### Troubleshooting

- **Check Service Status**:
  ```sh
  sudo systemctl status request.service
  ```

- **Check Journal Logs for Errors**:
  ```sh
  sudo journalctl -u request.service
  ```

- **Create the Log File Manually**:
  ```sh
  sudo touch /var/log/request.log
  sudo chmod 666 /var/log/request.log
  ```
