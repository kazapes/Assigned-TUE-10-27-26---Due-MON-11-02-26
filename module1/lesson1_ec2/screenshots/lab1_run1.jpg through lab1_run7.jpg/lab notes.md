## Run 1 — Instance ID: i-0315e3a096d1dcb02

I deployed Balerica Legacy Bank on the EC2 instance with public IP address `3.87.240.86` and opened the bank homepage and API documentation on port `8000`.
Looking up account `999999` returned `ERROR=ACCOUNT NOT FOUND` and exit code `4`, which demonstrated the application's handling of a nonexistent account.
The server logged a `404 Not Found` response for `/favicon.ico`, but the homepage and API documentation returned `200 OK`; my screenshots do not show a favicon fix.
I learned how to identify the Uvicorn PID and distinguish an individual failed request from an application startup failure.

Exact terminal output:

    INFO:     Started server process [37056]

## Run 2 — Instance ID: i-0865e3e2313ca5032

I repeated the deployment on another EC2 instance with public IP address `54.87.61.209` and private IP address `172.31.22.214`.
I confirmed that the instance details webpage displayed the new instance's information and that the bank API documentation loaded at `http://54.87.61.209:8000/docs`.
My screenshots show successful browser access but do not document a specific error or corrective command for this run.
I learned to match each deployment's public address and private instance information before recording its results.

Exact webpage output:

    Instance Private Ip Address: 172.31.22.214

Evidence gap: A terminal screenshot showing this run's PID or actual error message was not captured.

## Run 3 — Instance ID: i-02f8dbe9b9133ccc1

I moved to a third EC2 instance with public IP address `3.87.92.246` and private IP address `172.31.24.177`.
I verified that the instance details webpage loaded, the bank homepage displayed `FastAPI: ONLINE`, and the API documentation opened on port `8000`.
The supplied screenshots do not show an error or a fix for this run, so I have no specific troubleshooting action to report.
I learned to verify the instance details page, application homepage, and API documentation separately when checking a deployment.

Exact webpage output:

    Instance Private Ip Address: 172.31.24.177

Evidence gap: A terminal screenshot showing this run's PID or actual error message was not captured.

## Run 4 — Instance ID: i-06fdaa3fa28538294

I repeated the deployment on the EC2 instance with public IP address `18.212.76.52` and private IP address `172.31.16.18`.
I opened the instance details webpage, confirmed that the bank homepage displayed `FastAPI: ONLINE`, and accessed the API documentation at `http://18.212.76.52:8000/docs`.
No error or corrective action is visible in the screenshots for this run.
I learned that repeating the browser checks with the new instance's address helps confirm that I am viewing the intended deployment.

Exact webpage output:

    Instance Private Ip Address: 172.31.16.18

Evidence gap: A terminal screenshot showing this run's PID or actual error message was not captured.

## Run 5 — Instance ID: i-050ff705f5e2a05ed

I expanded testing on the instance with public IP address `34.227.57.88` by connecting through Git Bash, checking API endpoints, and running the SWIFT traffic generator.
My first SSH attempt used the private hostname `ip-172-31-24-63` and failed to resolve; another attempt used the incorrect hostname `ec2-34.227.57.88` and also failed.
I also answered `no` at the SSH host confirmation prompt, which ended that connection attempt with `Host key verification failed.`
I eventually connected using `ssh -i "MyBank5.pem" ec2-user@34.227.57.88` and answered `yes` at the confirmation prompt.
Uvicorn started with PID `36227`, and its logs showed payment requests returning both `422 Unprocessable Entity` and `200 OK`, although the exact request changes are not visible.
The SWIFT generator sent 10 payments: 8 were posted, 2 were rejected for insufficient funds, and 0 failed because of connectivity or server errors.
I learned to distinguish SSH address errors, API request validation failures, and expected banking rejections.

Exact terminal output:

    INFO:     Started server process [36227]

Exact SSH error:

    ssh: Could not resolve hostname ip-172-31-24-63: Name or service not known

## Run 6 — Instance ID: i-0f3fe3d2ca8e72296

I repeated the account lookup, payment posting, and application startup steps on the instance with public IP address `44.223.34.15` and private IP address `172.31.28.57`.
Account `100001` returned Alice Johnson's active account information and exit code `0`, while account `999999` returned `ERROR=ACCOUNT NOT FOUND` and exit code `4`.
The nonexistent-account result was an expected test outcome, so it did not require an application fix.
I posted payment `TXN0001` for `125.50 USD`, checked `data/payments.dat`, activated the virtual environment, and started Uvicorn with PID `37235`.
I then confirmed that the bank homepage and API documentation loaded in the browser.
I learned how the direct COBOL commands, saved payment record, and browser checks provide different evidence that the application is working.

Exact terminal output:

    INFO:     Started server process [37235]

## Run 7 — Instance ID: i-00341766914019641

I repeated the deployment checks on the instance with public IP address `3.89.29.236` and private IP address `172.31.18.68`.
The instance details webpage displayed the new private address, the bank homepage showed `FastAPI: ONLINE`, and the API documentation loaded at `http://3.89.29.236:8000/docs`.
The screenshots do not show a terminal error or corrective action for this run.
I learned to keep evidence tied to the correct Instance ID and IP addresses when documenting several deployments of the same application.

Exact webpage output:

    Instance Private Ip Address: 172.31.18.68

Evidence gap: A terminal screenshot showing this run's PID or actual error message was not captured.

## Mistakes I made

- I used the private hostname `ip-172-31-24-63` in an SSH attempt from my laptop, and it did not resolve.
- I entered `ec2-34.227.57.88` as a hostname, which was not the correct public IP address or public DNS name.
- I answered `no` at the SSH host confirmation prompt, which stopped that connection attempt.
- I did not capture terminal PID or error evidence for Runs 2, 3, 4, and 7.
- I did not record the request details needed to explain the `422 Unprocessable Entity` responses in Run 5.

## Commands I now know by heart

- `cd ~/Downloads`
- `ls -l MyBank5.pem`
- `chmod 400 MyBank5.pem`
- `ssh -i "MyBank5.pem" ec2-user@34.227.57.88`
- `cd /opt/balerica-bank`
- `ls`
- `./cobol/bin/account_lookup 100001`
- `./cobol/bin/account_lookup 999999`
- `echo $?`
- `./cobol/bin/payment_post TXN0001 100001 125.50 USD INV-1001`
- `cat data/payments.dat`
- `source venv/bin/activate`
- `uvicorn app.main:app --host 0.0.0.0 --port 8000`
- `curl http://127.0.0.1:8000/status`
- `curl http://127.0.0.1:8000/chewbacca`
- `python3 swift/traffic_generator.py --count 10`