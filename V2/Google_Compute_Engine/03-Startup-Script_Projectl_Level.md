
# Guide: Configuring a Project-Level Startup Script in Google Cloud

This guide provides instructions for setting up a project-level startup script that will apply to all VMs created in the project and configuring a new VM to use it.

---

### Step 1: Set the Project-Level Startup Script

1. **Go to the Google Cloud Console**.
2. Navigate to **Compute Engine** > **Settings**.
3. Select the **Metadata** tab.
4. Click **Edit** to add new metadata.
5. Click **Add Item**.
   - **Key**: Enter `startup-script`.
   - **Value**: Paste the following script:

    ```bash
    #!/bin/bash
    sudo apt install -y telnet
    sudo apt install -y nginx
    sudo systemctl enable nginx
    sudo chmod -R 755 /var/www/html
    HOSTNAME=$(hostname)
    sudo echo "<!DOCTYPE html>
    <html>
    <head>
        <style>
            body {
                background-color: #004d40;
                color: #ffffff;
                font-family: 'Arial', sans-serif;
                text-align: center;
                padding: 50px;
                margin: 0;
            }
            h1 { color: #ffab00; font-size: 2.5em; margin-bottom: 20px; }
            p { font-size: 1.2em; margin: 10px 0; }
            .hostname { color: #ffccbc; }
            .ip-address { color: #80deea; }
            .version { color: #ff5722; font-weight: bold; font-size: 1.5em; margin-top: 30px; }
            strong { color: #e0f2f1; }
        </style>
    </head>
    <body>
        <h1>i27academy Google Cloud Platform - Project Level Startup Script</h1>
        <p><strong class='hostname'>Server Hostname:</strong> ${HOSTNAME}</p>
        <p><strong class='ip-address'>Server IP Address:</strong> $(hostname -I)</p>
        <p class='version'>Version-1</p>
    </body>
    </html>" | sudo tee /var/www/html/index.html
    ```

6. Click **Save** to apply the changes.

This startup script will now be applied automatically to any new VM instance created within this project.

---

### Step 2: Create a New VM Instance

1. Go to **Compute Engine** > **VM Instances**.
2. Click **Create Instance**.
3. **Name**: Enter a name for the VM, like `project-level-vm`.
4. **Machine Type**: Leave this as **default** (e2-micro).
5. **Firewall Rules**: Optionally, enable **Allow HTTP traffic** to access the web server.
6. Leave all other settings as **default**.
7. Click **Create** to launch the VM.

The project-level startup script will run on this new VM, installing **telnet** and **nginx** and creating a custom HTML page with **Project Level Startup Script** text in the heading.

---

### Step 3: Access the Custom Webpage

1. Once the VM is created and running, go back to the **VM instances** page.
2. Copy the **External IP** of the VM.
3. Open a web browser and navigate to `http://[YOUR_VM_EXTERNAL_IP]`.

You should see the custom HTML page displaying **i27academy Google Cloud Platform - Project Level Startup Script**, along with the server’s hostname, IP address, and version.

---


---

### Note

Do not delete the project metadata, as the next example will demonstrate how to **override project-level metadata at the VM level**.

---