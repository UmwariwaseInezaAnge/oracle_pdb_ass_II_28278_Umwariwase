# Oracle Multitenant Architecture — Pluggable Database Assignment II

## 📌 Overview

This repository documents my work for Assignment II, covering:
- Creation of a new Pluggable Database (PDB) and a user inside it
- Creation and deletion of a temporary PDB
- Configuration and use of Oracle Enterprise Manager (OEM)
- Documentation of the process on GitHub

I created a pluggable database `um_pdb_28278` inside the `ORCL` container database and added a user
account (`UMW_PLSQAUCA_28278`) for coursework use. I then created and fully deleted a temporary PDB
(`um_to_delete_pdb_28278`) to practice the full PDB lifecycle, and worked with Oracle Enterprise Manager
to view the container database environment.

## 🖥️ Oracle Environment Used

- Oracle Database version: `21.3.0.0.0 Enterprise Edition`
- Container Database (CDB) name: `ORCL`
- Operating system: `Microsoft Windows x86 64-bit`
- Tools used: SQL*Plus, Oracle Enterprise Manager Database Express

## ✅ Task 1: Create a New Pluggable Database

**PDB Name:** `um_pdb_28278`
**Username created inside PDB:** `UMW_PLSQAUCA_28278`

### What I did
I created the pluggable database `um_pdb_28278` and created the user `UMW_PLSQAUCA_28278` inside it. I
verified the user with a query against `dba_users`, which confirmed the account status as `OPEN`.

### Evidence
- `screenshots/pdb_creation/user_created.png` — User verified inside PDB, account status `OPEN`
- The OEM dashboard screenshot under Task 3 also shows `um_PDB_28278` active in the Containers view, corroborating that the PDB exists and is running.

### ⚠️ Evidence gap
I do not have a dedicated screenshot of the `CREATE PLUGGABLE DATABASE um_pdb_28278` command itself or a
standalone `ALTER PLUGGABLE DATABASE um_pdb_28278 OPEN` / `v$pdbs` open-state check. The OEM Containers
view (see Task 3) confirms the PDB exists and is running, which partially offsets this, but the exact
requested screenshots are missing. I'm submitting with this gap noted rather than not submitting at all.

### ⚠️ Naming note
The username in my evidence reads `UMW_PLSQAUCA_28278`, which does not exactly match the required
`FirstName_plsqlauca_StudentID` format (missing the "L" in "plsql", and using `UMW` rather than my full
first name). I'm flagging this for the record rather than silently leaving it.

## ✅ Task 2: Create and Delete a PDB

**Temporary PDB Name:** `um_to_delete_pdb_28278`

### What I did
I created the temporary pluggable database `um_to_delete_pdb_28278` with the admin user `tempadmin`,
using `FILE_NAME_CONVERT` to map it from the `PDBSEED` files. I verified it existed by querying `v$pdbs`,
which showed it in `MOUNTED` state. I then ran `DROP PLUGGABLE DATABASE um_to_delete_pdb_28278 INCLUDING
DATAFILES` to delete it completely, and re-ran the same `v$pdbs` query, which returned `no rows
selected` — confirming the PDB and its datafiles were fully removed.

I separately opened the temporary PDB with `ALTER PLUGGABLE DATABASE um_to_delete_pdb_28278 OPEN`,
initially hitting `ORA-01031: insufficient privileges`, then reconnecting as `SYS AS SYSDBA` before the
command succeeded and the PDB showed `READ WRITE`.

### Evidence
- `screenshots/pdb_deletion/pdb_creation_and_deletion.png` — Creation command, existence check, deletion command, and final verification (no rows selected)
- `screenshots/pdb_deletion/temp_pdb_open_state.png` — Opening the temp PDB, privilege error, fix via SYSDBA, confirmed READ WRITE

## ✅ Task 3: Oracle Enterprise Manager (OEM)

### What I did
I accessed Oracle Enterprise Manager Database Express, logged in as `system`, and opened the Database
Home page for the CDB instance (version 21.3.0.0.0 Enterprise Edition). The Containers performance view
shows `um_PDB_28278` and `ORCLPDB2` active within the CDB, reflecting the PDB created in Task 1.

### Evidence
- `screenshots/oem_dashboard/oem_dashboard.png` — OEM Database Home dashboard showing `um_PDB_28278` active in the Containers view, `system` user visible top-right

## ⚠️ Challenges Faced

- Ran into `ORA-01031: insufficient privileges` when trying to open a PDB while connected with a
  lower-privileged account. Resolved by reconnecting with `CONNECT SYS AS SYSDBA` before running the
  `ALTER PLUGGABLE DATABASE ... OPEN` command.
- Ran short on time before capturing a dedicated `CREATE PLUGGABLE DATABASE` / open-state screenshot for
  `um_pdb_28278` specifically (see Task 1 evidence gap above); the OEM dashboard partially offsets this
  by showing the PDB active.

## 🔒 Integrity Statement

I confirm that all work in this repository was completed individually by me, without collaboration,
copied material, or AI-generated commands or solutions. All screenshots reflect my own execution of the
assignment; gaps in evidence are disclosed above rather than fabricated.

## 📤 Submission Details

- **Repository Link:** https://github.com/UmwariwaseInezaAnge/oracle_pdb_ass_II_28278_Umwariwase
- **PDB Name Created:** `um_pdb_28278`
- **Issues Encountered:** `Yes — see Challenges Faced and Evidence gap notes above`
