# Assignment II: Oracle Pluggable Database (PDB) Management


| Student | Umwariwase INEZA Ange |
| Student ID | 28278 |
| Course | Database Development with PL/SQL (INSY 8311), AUCA |
| Repository | oracle_pdb_ass_II_28278_umwariwase |

 Contents

1. [Overview of Tasks](#1-overview-of-tasks)
2. [Oracle Environment Used](#2-oracle-environment-used)
3. [Explanation of Each Task](#3-explanation-of-each-task)
4. [Challenges Faced and Solutions](#4-challenges-faced-and-solutions)
5. [Integrity Statement](#5-integrity-statement)
6. [Submission Details](#6-submission-details)
7. [Repository Structure](#7-repository-structure)
8. [Screenshots (Evidence)](#8-screenshots-evidence)


 1. Overview of Tasks

| Task | Description | Result |
|------|-------------|--------|
| 1 | Create a new pluggable database and a user inside it | PDB `um_pdb_28278` created, open in `READ WRITE`; user `UMWARIWASE_PLSQLAUCA_28278` created inside it |
| 2 | Create a temporary PDB, verify it, delete it completely, confirm it is gone | `um_to_delete_pdb_28278` created and dropped with its datafiles |
| 3 | Access Oracle Enterprise Manager and capture the dashboard | EM Express dashboard captured, showing the PDB and the logged-in username |
| 4 | Document the work and publish it on GitHub | This repository |
 2. Oracle Environment Used

| Item | Value |
|------|-------|
| Database | Oracle Database 21c Express Edition (21.3.0.0.0) |
| Operating system | Microsoft Windows 64-bit |
| Container database (CDB) | `XE` |
| Existing default PDB (not modified) | `XEPDB1` |
| Tools | SQL Developer, SQL*Plus, Oracle Enterprise Manager Database Express |
| Listener | `localhost:1521` |
| EM Express URL | `https://localhost:5500/em` |

 3. Explanation of Each Task

 Task 1: Create a New Pluggable Database

| Item | Value |
|------|-------|
| PDB name | `um_pdb_28278` |
| User inside the PDB | `Umwariwase_plsqlauca_28278` (stored by Oracle as `UMWARIWASE_PLSQLAUCA_28278`) |

Commands were run as `SYS` with the `SYSDBA` role, connected to the `XE` service (root container `CDB$ROOT`):

sql
CREATE PLUGGABLE DATABASE um_pdb_28278
  ADMIN USER pdb_admin IDENTIFIED BY <admin_password>
  FILE_NAME_CONVERT = ('C:\APP\SMILE\PRODUCT\21C\ORADATA\XE\PDBSEED\',
                       'C:\APP\SMILE\PRODUCT\21C\ORADATA\XE\UM_PDB_28278\');

ALTER PLUGGABLE DATABASE um_pdb_28278 OPEN;
ALTER PLUGGABLE DATABASE um_pdb_28278 SAVE STATE;
SHOW PDBS


The user was then created inside the PDB, not in the root:

sql
ALTER SESSION SET CONTAINER = um_pdb_28278;
CREATE USER Umwariwase_plsqlauca_28278 IDENTIFIED BY <password>;
GRANT CREATE SESSION TO Umwariwase_plsqlauca_28278;
GRANT CONNECT, RESOURCE, DBA TO Umwariwase_plsqlauca_28278;
GRANT UNLIMITED TABLESPACE TO Umwariwase_plsqlauca_28278;
```

Verification. This query confirms the PDB, its open mode and the user together:sql
SELECT p.name AS pdb_name, p.open_mode, u.username, u.account_status
FROM   v$pdbs p
JOIN   cdb_users u ON u.con_id = p.con_id
WHERE  u.username LIKE '%PLSQLAUCA%';


Result: `UM_PDB_28278`, `READ WRITE`, `UMWARIWASE_PLSQLAUCA_28278`.

 Task 2: Create and Delete a PDB

Temporary PDB name: `um_to_delete_pdb_28278`

The PDB was created with the same method as Task 1 and checked with `SHOW PDBS`. It was then removed completely:

sql
ALTER PLUGGABLE DATABASE um_to_delete_pdb_28278 CLOSE IMMEDIATE;
DROP PLUGGABLE DATABASE um_to_delete_pdb_28278 INCLUDING DATAFILES;
SHOW PDBS


`INCLUDING DATAFILES` also deletes the PDB's files from disk. The final `SHOW PDBS` listed only `PDB$SEED`, `XEPDB1` and `UM_PDB_28278`, which confirms the temporary PDB no longer exists and the main PDB was not affected.

 Task 3: Oracle Enterprise Manager (EM Express)

EM Express was opened at `https://localhost:5500/em` and logged in with the PDB user `UMWARIWASE_PLSQLAUCA_28278` and container `um_pdb_28278`. The Database Home page shows:

- the environment `XE / UM_PDB_28278 (21.3.0.0.0)`
- the instance status (Single Instance, Express Edition, Windows 64-bit)
- the logged-in username in the top right corner
 4. Challenges Faced and Solutions

| Challenge | Cause | Solution |
|-----------|-------|----------|
| `ORA-01031: insufficient privileges` when dropping a PDB | Connected through the `XEPDB1` service as a normal user instead of the root | Reconnected as `SYS` with role `SYSDBA` on service `XE` and confirmed `CON_NAME = CDB$ROOT` |
| `SP2-0158: unknown SHOW option` | Added `;` and `--` comments on `SHOW` lines in SQL*Plus | Ran `SHOW CON_NAME` alone on its line, without comments |
| `ORA-01017: invalid username/password` in SQL Developer | The username field contained `_2026` instead of `_28278` | Proved the login in SQL*Plus first, then corrected the username and used **Service name** `um_pdb_28278` instead of SID |
| Locked user account | Repeated failed login attempts | `ALTER USER ... ACCOUNT UNLOCK` inside the PDB |
| `ORA-65019: pluggable database already open` | The open command was run on a PDB that was already open | Checked `OPEN_MODE` with `SHOW PDBS`; no action needed |
| `ORA-65020: pluggable database already closed` | The temporary PDB was already closed before the drop | Ran `DROP PLUGGABLE DATABASE`, which completed successfully |

 5. Integrity Statement

I confirm that this work is my own. I carried out every step on my own Oracle environment, and all screenshots in this repository were taken on my machine. I understand the commands used and can explain them.

 6. Submission Details



- Student name:umwariwase ineza ange
- Student ID: 28278
- GitHub repository: (https://github.com/UmwariwaseInezaAnge/oracle_pdb_ass_II_28278_Umwariwase.git)

 7. Repository Structure


oracle_pdb_ass_II_28278_umwariwase/
├── README.md
└── screenshots/
    ├── pdb_creation/
    ├── pdb_deletion/
    └── oem_dashboard/
```

8. Screenshots (Evidence)

 PDB Creation (Task 1)

PDB creation command and result
![PDB creation](screenshots/pdb_creation/01_create_um_pdb_28278.png)

PDB open state (`READ WRITE`)
![PDB open state](screenshots/pdb_creation/02_pdb_open_state.png)
User created inside the PDB
![User created in PDB](screenshots/pdb_creation/03_user_created_in_pdb.png)

Combined verification of PDB, open mode and user
![Verification query](screenshots/pdb_creation/04_verification_query.png)
 PDB Deletion (Task 2)

Temporary PDB creation command and result
![Temporary PDB created](screenshots/pdb_deletion/05_create_temp_pdb.png)

Temporary PDB deletion command and result**
![Temporary PDB dropped](screenshots/pdb_deletion/06_drop_temp_pdb.png)

Confirmation that the temporary PDB no longer exists
![Deletion confirmed](screenshots/pdb_deletion/07_confirm_pdb_deleted.png) OEM Dashboard (Task 3)

![OEM dashboard](screenshots/oem_dashboard/08_oem_dashboard.png)
