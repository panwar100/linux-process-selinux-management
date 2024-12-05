# linux-process-selinux-management
# Linux Process Management and SELinux Configuration
This guide provides a comprehensive explanation of managing Linux process priorities using ps, jobs, nice, and renice, alongside an in-depth overview of SELinux (Security-Enhanced Linux), its modes, configurations, and its role in securing system resources like files, folders, and ports.

# Table of Contents

[1.Process Management](#process-management)

- [View Processes](#view-processes)
- [Understanding PRI and NI](#understanding-pri-and-ni)
- [Changing Process Priority with Nice](#changing-process-priority-with-nice)
- [Modifying Priority of a Running Process with Renice](#modifying-priority-of-a-running-process-with-renice)

[2.SELinux Overview](#selinux-overview)

- [What is SELinux?](#what-is-selinx)
- [SELinux Enforcement Modes](#selinux-enforcement-modes)
- [Disabling SELinux](#disabling-selinux)
- [SELinux Security Scope](#selinux-security-scope)

# 1.Process Management
## A.View Processes

**a)Command**:

![Screenshot from 2024-12-05 20-49-17](https://github.com/user-attachments/assets/59cdabea-a311-4591-9a8b-6c88345e403b)

Displays a list of running processes for the current user.

---
**b)View Background Jobs**:

![Screenshot from 2024-12-05 20-49-55](https://github.com/user-attachments/assets/c6fbf578-382a-4802-a4cc-c65a30961a24)


Displays the current background jobs and their status.

---
**c)Detailed Process List**:

![Screenshot from 2024-12-05 20-50-33](https://github.com/user-attachments/assets/b3dd41f1-71ce-421f-baa1-b59c54f11e94)


Shows detailed information about processes, including:

- PRI (Priority): Determines the scheduling priority of a process.
- NI (Nice value): A user-defined adjustment for process priority.

## B.Understanding PRI and NI

**PRI (Priority)**:

- Range: 0 to 139.
- Real-time processes: Priority 0–99 (higher priority).
- Normal processes: Priority 100–139 (lower priority).
- A lower PRI value indicates a higher priority.
---

**NI (Nice Value)**:

- Range: -20 to 19.
- Negative NI: Increases priority (e.g., -20).
- Positive NI: Decreases priority (e.g., 19).
- The NI value adjusts the priority by influencing the PRI value.
---

**Key Points**:

- **a)PRI and NI Relationship**: PRI=20+NI(for normal processes)

- **b)Real-Time Processes**: Real-time priorities range from 0–99 and are not influenced by NI.

- **c)Effect of NI**:
  - Negative NI → Higher priority.
  - Positive NI → Lower priority.

- **d)Default NI**: The default NI value is 0.

By adjusting the NI value, you indirectly control the PRI and influence how the scheduler prioritizes a process.

## C.Changing Process Priority with Nice

a) Start a process with a specific nice value:

`nice -n <value> <command>`

Example:

![Screenshot from 2024-12-05 20-52-48](https://github.com/user-attachments/assets/fc223d2a-43e8-4f0f-8856-ac88379316ce)

Starts the sleep command with a nice value of 10, reducing its priority.

b) Check Priority:

![Screenshot from 2024-12-05 20-53-40](https://github.com/user-attachments/assets/281a1743-2ad6-46ba-bfad-f02450406523)

Observe the PRI and NI values for the process.

## D.Modifying Priority of a Running Process with Renice

a) Change Priority of an Existing Process:

`renice -n <nice_value> -p <PID>`

Example:

![Screenshot from 2024-12-05 20-55-34](https://github.com/user-attachments/assets/d4190df4-e1be-425b-a871-f62c7226294e)

Sets the nice value of the process with PID 2717 to -20, giving it higher priority.

b) Verify Changes:

![Screenshot from 2024-12-05 20-56-01](https://github.com/user-attachments/assets/dc9e866e-aee7-4ca1-bebf-cb2b8b916ac2)

The updated PRI and NI values will reflect in the process list.

# 2.SELinux Overview

## A.What is SELinux?
Security-Enhanced Linux (SELinux) is a Linux kernel security module that provides mandatory access control (MAC). Unlike traditional discretionary access controls (DAC), SELinux enforces strict security policies on processes, files, folders, and network ports, regardless of user permissions.

**Key Features of SELinux**:

- **Enhanced Security**: SELinux restricts applications and processes to operate only within their designated security contexts.
- **Prevention of Unauthorized Access**: Policies ensure that even root users cannot bypass restrictions.
- **Granular Control**: SELinux policies allow fine-grained control over access to files, directories, and ports.

## B.SELinux Enforcement Modes

### a) Check Current SELinux Mode:

![Screenshot from 2024-12-05 21-08-30](https://github.com/user-attachments/assets/7c85d115-d71d-4e09-8d33-2fa40891ef22)

---
### b) Outputs the current SELinux mode:

**Enforcing**: Enforces SELinux policies.

**Permissive**: Logs policy violations without enforcing them.

**Disabled**: SELinux is turned off.

---
### c) Switch Between Modes:

- **To set SELinux to permissive mode (logs only)**:

![Screenshot from 2024-12-05 21-10-12](https://github.com/user-attachments/assets/e05c12d1-409c-44fe-9a02-aaad435b356e)


- **To enforce SELinux policies**:

![Screenshot from 2024-12-05 21-10-38](https://github.com/user-attachments/assets/99a3ad4b-2765-4271-baf5-6abca28afb14)


## C.Disabling SELinux

a) Temporarily Disable SELinux: Use setenforce to switch to permissive mode or disable SELinux temporarily without a reboot:

![Screenshot from 2024-12-05 21-10-12](https://github.com/user-attachments/assets/e05c12d1-409c-44fe-9a02-aaad435b356e)

---
b) Permanently Disable SELinux:

**Edit the SELinux configuration file**:

vim /etc/selinux/config

**Update the following line**:

![Screenshot from 2024-12-05 21-12-34](https://github.com/user-attachments/assets/ec2b93d0-1398-4ed1-b8c0-e1f128c9a430)

---
c) Restart the system for the changes to take effect:

reboot

Warning: Disabling SELinux removes its protections, exposing the system to potential vulnerabilities. Use with caution.

## D.SELinux Security Scope
SELinux provides security for the following:

- **Folders**:
SELinux ensures that directories have proper security contexts. For example, a web server process can only access web-related folders.

- **Files**:
Each file is assigned a security context, preventing unauthorized access even by privileged users.

- **Ports**:
SELinux restricts network services to operate only on designated ports, ensuring safe communication.

---
### Example Usage

- Protecting Files and Folders:

a) Set appropriate security contexts for files and folders:
 
chcon -t httpd_sys_content_t /var/www/html

b) Restore default contexts if needed:

restorecon -R /var/www/html

- Managing Ports:

a) Allow an application to bind to a specific port:

semanage port -a -t http_port_t -p tcp 8080

### Notes
- Use getenforce and setenforce for temporary mode changes without rebooting.
- SELinux policies are highly customizable; explore /etc/selinux for configuration files.
