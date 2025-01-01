# **Detection and Response Workflow with LimaCharlie, LaZagne, Slack, and Tines**

## **Objective**
This project focuses on creating an automated detection and response workflow using LimaCharlie, LaZagne, Slack, and Tines. It demonstrates how to set up a Windows Server environment for monitoring and detecting credential dumping attacks, specifically using LaZagne, an open-source credential-dumping tool. The workflow integrates various tools to monitor activity, generate alerts, and automate responses in a cybersecurity context.

## **Skills Learned**
- Understanding of modern detection and response workflows in cybersecurity.
- Proficiency in using LimaCharlie for monitoring and creating detection rules.
- Hands-on experience with LaZagne for simulating credential dumping.
- Integration of Slack and Tines for automated alerting and response.
- Practical experience with generating logs and creating detection rules for common attack scenarios.

## **Tools Used**
- **LimaCharlie**: A cloud-based cybersecurity platform for monitoring and detection.
- **LaZagne**: An open-source tool for credential dumping to simulate attacks.
- **Slack**: A communication tool for receiving alerts and notifications.
- **Tines**: An automation platform to manage responses based on user input and system events.
- **PowerShell**: Used to execute LaZagne on the Windows Server machine.

## **Steps**

### **1. Setting Up the Environment**
The project begins by setting up a Windows Server environment and installing LimaCharlie to monitor system activity.

#### Key Tools and Configurations:
- **Windows Server**: Deployed as the core machine for testing.
- **LimaCharlie Agent**: Installed and configured to monitor server activity.
- **Validation**: Confirm the server is visible and functional in the LimaCharlie dashboard.

![Windows Server Setup](imgsrc)  
*Ref 1: Windows Server Setup Screenshot*  
*Screenshot of the Windows Server environment being prepared for the detection and response workflow.*

### **2. Exploring LaZagne for Credential Dumping**
The project uses LaZagne, a credential-dumping tool, to simulate an attack on the Windows Server. Understanding LaZagne’s behavior is essential for crafting precise detection rules.

#### Steps I Followed:
1. **Download and Install LaZagne**: Install LaZagne from its official GitHub repository.
2. **Execute LaZagne**: Run LaZagne via PowerShell to simulate credential dumping on the Windows Server.
3. **Analyze Activity**: Capture the activity to understand its behavior and create detection rules.

![LaZagne Execution](imgsrc)  
*Ref 2: LaZagne Execution Screenshot*  
*Screenshot of LaZagne being executed on the Windows Server.*

### **3. Generating Telemetry and Crafting Detection Rules**
After executing LaZagne, the next step is to generate logs and create detection rules in LimaCharlie to identify the tool’s activity based on system behavior.

#### Key Configurations:
- **Detection Rules**: Create rules to detect LaZagne-specific signatures such as file paths, command-line usage, and hash values.
  - **Example Rule**:
    ```yaml
    events:
      - NEW_PROCESS
      - EXISTING_PROCESS
    op: and
    rules:
      - op: is windows
      - op: or
        rules:
          - case sensitive: false
            op: ends with
            path: event/FILE_PATH
            value: LaZagne.exe
          - case sensitive: false
            op: contains
            path: event/COMMAND_LINE
            value: LaZagne
          - case sensitive: false
            op: is
            path: event/HASH
            value: '3cc5ee93a9ba1fc57389705283b760c8bd61f35e9398bbfa3210e2becf6d4b05'
    action: report
    metadata:
      author: aadhav
      description: Detects LaZagne Usage
      falsepositives:
      - ToTheMoon
      level: high
      tags:
      - attack.credential_access
    name: aadhav - HackTool - Lazagne
    ```

![Detection Rule](imgsrc)  
*Ref 3: Detection Rule Screenshot*  
*Screenshot of the custom detection rule created in LimaCharlie to detect LaZagne usage.*

### **4. Alerting via Slack and Tines Integration**
Once the detection rule is created, integrate Slack and Tines for automated alerts.

#### Key Steps:
1. **Set Up Slack**: Create a Slack account and set up a dedicated channel for alerts.
2. **Integrate Tines**: Create a webhook URL in Tines and connect it to LimaCharlie to send alerts to Slack.
3. **Test the Setup**: Simulate a detection event to ensure that alerts are received in Slack.

![Slack Integration](imgsrc)  
*Ref 4: Slack Integration Screenshot*  
*Screenshot of Slack receiving alerts triggered by LimaCharlie.*

### **5. Automated Response with User Interaction**
The workflow allows users to respond dynamically based on alerts received in Slack.

#### Key Steps:
1. **User Interaction**: After a detection event, Slack will prompt the user with options to isolate the machine.
2. **Machine Isolation**: If the user selects ‘Yes,’ the system isolates the machine via LimaCharlie and notifies the user.
3. **Further Inspection**: If ‘No’ is selected, the user is notified that further inspection is required.

![Automated Response](imgsrc)  
*Ref 5: Automated Response Screenshot*  
*Screenshot of the Slack channel displaying alerts and user prompts for automated responses.*

## **Lessons Learned**
This project was a valuable exercise in automating detection and response workflows using modern cybersecurity tools. Key takeaways include:
- The importance of having real-time detection rules to identify malicious activity.
- How integrating communication tools like Slack can enhance incident response.
- The role of automation platforms such as Tines in reducing response times and improving efficiency.

## **Final Thoughts**
This project highlights the effectiveness of combining detection platforms, automation, and communication tools to create a comprehensive cybersecurity workflow. It provides an adaptable framework for detecting and responding to credential dumping attacks, which is crucial in today’s evolving threat landscape.
