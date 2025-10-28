# Jenkins Master–Slave Setup Using ngrok Free Service (Same Network)

## Aim
To set up Jenkins Master–Slave (Agent) architecture and access Jenkins Master web UI remotely using ngrok's free HTTPS service, with both nodes on the same network.

## Requirements
- Two machines (or VMs) on the same local network:
  - Jenkins Master (Controller)
  - Jenkins Slave (Agent)
- Java installed on both machines
- ngrok installed on Jenkins Master
- Jenkins installed on Master

---

## Procedure

### 1. Install Jenkins Master
- On the Master machine:
  - Download Jenkins from [jenkins.io](https://www.jenkins.io/download/)
  - Install Jenkins and start the service
  - Access Jenkins dashboard at `http://localhost:8080`

### 2. Install ngrok on Jenkins Master
- Download ngrok from [ngrok.com](https://ngrok.com/download)
- Unzip and place `ngrok.exe` in a known location
- Open a terminal and run:
  ```sh
  ngrok http 8080
  ```
- Copy the HTTPS forwarding URL (e.g., `https://xxxx.ngrok-free.app`)

### 3. Prepare Jenkins Slave (Agent)
- On the Slave machine:
  - Ensure Java is installed (`java -version`)
  - No need for ngrok on Slave

### 4. Configure Slave Node in Jenkins
- On Jenkins Master dashboard (access via ngrok URL):
  - Go to **Manage Jenkins > Manage Nodes and Clouds > New Node**
  - Enter a name (e.g., `slave1`), select **Permanent Agent**, click **OK**
  - Configure:
    - Remote root directory (e.g., `/home/jenkins` or `C:\jenkins`)
    - Launch method: **Launch agent by connecting to the master** (JNLP)
    - Download `agent.jar` from Jenkins
    - On Slave, run:
      ```sh
      java -jar agent.jar -jnlpUrl https://xxxx.ngrok-free.app/computer/slave1/slave-agent.jnlp -secret <secret>
      ```
    - Use the ngrok HTTPS URL for `<master-ip>`

### 5. Verify Connection
- In Jenkins dashboard, the Slave node should show as **Online**

### 6. Run a Sample Build Job on Slave
- Create a new **Freestyle Project** or **Pipeline** job
- In job config, restrict build to Slave node label
- Add a simple build step (e.g., `echo "Hello from Slave"`)
- Build and check logs

---

## Expected Outcome
- Jenkins Master accessible via ngrok HTTPS URL
- Slave node connects and runs jobs
- Distributed build execution is demonstrated

---

## Notes
- ngrok free service only exposes HTTP/HTTPS ports, not TCP
- Both machines must be able to reach each other on the local network
- Use the ngrok URL for all remote access to Jenkins Master
