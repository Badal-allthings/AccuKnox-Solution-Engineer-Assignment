\# AccuKnox Solution Engineer Trainee Assignment

\## Problem Statement 2: Kubernetes Deployment and DVWA Security Testing



\### 1. Objective

Deploy a local Kubernetes cluster, run Damn Vulnerable Web Application (DVWA), and demonstrate three common web application security vulnerabilities in an isolated lab environment.



\### 2. Environment and Tools

\- Operating System: Windows 11

\- Container Runtime: Docker Desktop

\- Kubernetes: Minikube

\- Application: DVWA (`vulnerables/web-dvwa`)

\- Namespace: `dvwa-lab`

\- Deployment: `dvwa`

\- Service: `dvwa` (ClusterIP)

\- Local access: `http://127.0.0.1:8080`



\### 3. Kubernetes Setup



Start the local Kubernetes cluster:



```powershell

minikube start --driver=docker

```



Create the namespace:



```powershell

minikube kubectl -- create namespace dvwa-lab

```



Deploy DVWA using the Kubernetes manifest:



```powershell

minikube kubectl -- apply -f dvwa.yaml

```



Verify the deployment, pod, and service:



```powershell

minikube kubectl -- -n dvwa-lab get deployments,pods,services

```



Forward the service to the local machine:



```powershell

minikube kubectl -- -n dvwa-lab port-forward service/dvwa 8080:80

```



Keep the port-forward terminal running and open `http://127.0.0.1:8080` in a browser. Log in to DVWA with the lab's default credentials and complete the database setup if prompted.



\### 4. Security Demonstrations



Testing was performed against the local DVWA instance with its security level set to \*\*Low\*\*.



\#### Test 1: SQL Injection



\- \*\*Location:\*\* DVWA → SQL Injection

\- \*\*Test input:\*\* `1' OR '1'='1' #`

\- \*\*Observation:\*\* The application returned multiple records rather than only the record expected for a normal ID lookup.

\- \*\*Security impact:\*\* Improperly constructed SQL queries can expose database records and, depending on the query and database permissions, enable further database manipulation.

\- \*\*Mitigation:\*\* Use parameterized queries or prepared statements, validate inputs on the server, apply least-privilege database permissions, and avoid exposing sensitive database errors.

\- \*\*Evidence:\*\* `evidence/sql-injection.png`



\#### Test 2: Reflected Cross-Site Scripting (XSS)



\- \*\*Location:\*\* DVWA → XSS (Reflected)

\- \*\*Test input:\*\* `<script>alert('XSS')</script>`

\- \*\*Observation:\*\* A browser alert appeared, demonstrating that user-supplied JavaScript was executed by the application.

\- \*\*Security impact:\*\* Reflected XSS can execute attacker-controlled scripts in a victim's browser and may enable unauthorized actions or exposure of accessible page data.

\- \*\*Mitigation:\*\* Apply context-aware output encoding, validate and sanitize input where appropriate, and use a suitable Content Security Policy (CSP).

\- \*\*Evidence:\*\* `evidence/xss-reflected.png`



\#### Test 3: OS Command Injection



\- \*\*Location:\*\* DVWA → Command Injection

\- \*\*Test input:\*\* `127.0.0.1; id`

\- \*\*Observation:\*\* The output included user and group information for `www-data`, demonstrating that the injected operating-system command executed.

\- \*\*Security impact:\*\* Command injection can allow unauthorized commands to run with the privileges of the application process.

\- \*\*Mitigation:\*\* Avoid constructing shell commands from user input, use safe APIs instead of shell execution where possible, strictly validate expected input formats, and run the application with minimal privileges.

\- \*\*Evidence:\*\* `evidence/command-injection.png`



\### 5. Evidence



The `evidence` directory contains screenshots documenting:



1\. `kubernetes-deployment.png` — Kubernetes Deployment, Pod, and Service status.

2\. `sql-injection.png` — SQL Injection input and observed output.

3\. `xss-reflected.png` — Reflected XSS demonstration.

4\. `command-injection.png` — Command Injection demonstration.



\### 6. Security Considerations



All vulnerability demonstrations were performed against a deliberately vulnerable application deployed locally for educational purposes. The tests were not directed at public websites or systems belonging to other parties.



The lab illustrates why secure query construction, output encoding, safe process execution, least privilege, and layered application and infrastructure security controls are important.



\### 7. Conclusion



Successfully deployed DVWA on a local Minikube Kubernetes cluster and demonstrated SQL Injection, Reflected Cross-Site Scripting, and OS Command Injection. Documented the observed results, potential security impact, mitigation strategies, and supporting evidence for each demonstration.

