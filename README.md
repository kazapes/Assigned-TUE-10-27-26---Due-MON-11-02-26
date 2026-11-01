# Assigned-TUE-10-27-26---Due-MON-11-02-26

# WEEK 3 ASSIGNMENTS


![Image alt](https://github.com/kazapes/Assigned-TUE-10-27-26---Due-MON-11-02-26/blob/2bbd47323696e4ff6e53e6860cb09da911109b97/capetown2026.jpg)
# Group 
## Wolfpak
### Members
#### T.I.Q.S. 3rd3y3 Michael Maximus Uriah_Victorious


## Overview
- Lecture on migrating a legacy application ("Valerica Legacy Bank") into containerized environments.
- Instructor demonstrates building and launching an EC2-based application using AWS Launch Templates and a provided runbook.
- Emphasis on following the runbook step-by-step, copying commands precisely, and asking immediate questions when unclear.
- Target outcome: run the bank web app accessible on port 8000.

## Preparation And Goals
- Main objective: migrate legacy systems into containerized environments and learn SEER foundations with generative AI assistance.
- Use the provided runbook and the instructor's GitHub repo for lab steps and code.
  - Students must fork and sync the instructor’s repo before class.
  - The runbook contains copy-paste commands for reproducible setup.

## AWS Region And Launch Template Setup
- Ensure you are operating in the correct AWS region (Virginia in class).
- Create or modify a Launch Template; instructor used a template named "Basic App" (example).
- Key Launch Template settings to confirm:
  - Amazon Linux AMI (instructor prefers Amazon Linux).
  - Instance type with approximately 2 GB memory (example: t2.small or t3 with 2 GB).
  - User data (startup script) must be included in Advanced Details.
  - Template tags: add a Name tag (e.g., "my-bank") for clarity.

## Instance Type And Memory Requirement
- Critical requirement: the instance must have ~2 GB RAM.
  - Instructor does not care about exact instance type name; only the memory matters.
  - Acceptable examples: t2.small, certain t3 types — verify memory (2 GB) in selection list.

## Key Pair And Security Groups
- Key Pair:
  - Designate a key pair at instance creation; the instance expects that key for SSH access.
  - Cannot assign or switch to a different key pair for an existing instance after launch.
  - Best practice for labs: use the same key pair for multiple instances (simple approach).
- Security Group inbound rules required for this application:
  - SSH (port 22)
  - HTTP (port 80)
  - Custom TCP port 8000 (application-specific)
- Security Group source for class exercises:
  - Use Anywhere (0.0.0.0/0) for IPv4 as the class default.

## Launching Instance From Template
- Use Launch Templates -> Actions -> Launch instance from template -> select template version.
- If "Launch name already in use" error occurs, read the error message and resolve per instructions.
- Wait for instance initialization to complete before connecting.

## Connecting And Verifying Instance
- Connect to EC2 via SSH using the designated key pair and the EC2 "ec2-user" account (example).
- Verify working directory and identity:
  - whoami -> should show ec2-user
  - pwd -> should show /home/ec2-user

## Repository, Git, And Code Deployment
- Fork and sync the instructor’s GitHub repo before class; new code may be added weekly.
- Install Git on the EC2 instance (required to clone repo).
- Typical workflow on EC2:
  - Install Git (e.g., sudo yum install -y git or provided command)
  - git clone <instructor-repo-url>
  - cd into the cloned repo and list files (ls) to confirm contents match runbook.
- Run the provided installer script on EC2 (copy-paste from runbook).
  - Expect package install output and possible pip upgrade warnings.
  - If pip upgrade recommended, run the shown pip upgrade command exactly.

## Application Installation And Startup
- After cloning and running install script:
  - Verify database and example data (instructor checked an "Alice Johnson" account).
  - Start the bank API server using commands from runbook (copy-paste).
  - Start the web server; the application listens on port 8000 by default.
  - Port 8000 must be open in the security group for the app to be reachable.

## Troubleshooting Tips
- Read error messages carefully — they often indicate the problem (e.g., "launch name already in use", instance type limits).
- Common issues and fixes:
  - Cannot SSH: verify correct key pair used at instance creation and correct security group port 22.
  - App unreachable on :8000: confirm security group includes port 8000 and server process started.
  - Instance type launch blocked: error like "account cannot launch T2 instances with unlimited enabled" -> inspect error text and ask AI or support for clarifications.
  - If runbook files not present locally, refresh or resync your forked repo.
- Do not run ahead of documentation; follow runbook steps sequentially to avoid mistakes.

## Key Concepts And Analogies
- Key pair analogy: SSH key pair is like a lock and key for your EC2 instance; the public IP is the house address (front-facing), while SSH access is entry to the interior (protected).
- Importance of templates: Use Launch Templates and automation (Terraform) to reproduce environments reliably.

## Action Items / Next Steps
- Fork and sync the instructor’s GitHub repo; refresh before each class.
- Create or update Launch Template with:
  - Amazon Linux AMI
  - Instance type with 2 GB RAM
  - User data (startup script)
  - Name tag
- Create security group allowing ports 22, 80, and 8000 from 0.0.0.0/0.
- Launch instance from the template, SSH in with designated key pair, and follow runbook commands (copy-paste).
- Start API and web server; verify web app at http://<public-dns-or-ip>:8000
- If errors occur, capture full error text and consult runbook, classmates, instructor, or AI for explanation.

## Key Terms And Definitions
| *Term* | *Definition* |
|---|---|
| Launch Template | AWS resource that stores instance configuration for repeatable EC2 launches. |
| Key Pair | SSH public/private key pair used to authenticate SSH access to EC2 instances. |
| Security Group | AWS virtual firewall controlling inbound/outbound traffic to EC2 instances. |
| User Data | Script or commands executed at instance launch (placed in Advanced Details). |
| Git Clone | Command to copy a remote repository to the local machine. |
| Public DNS / Public IP | External address for accessing EC2-hosted services (front-end reachable publicly). |
