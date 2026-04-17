---
{"dg-publish":true,"permalink":"/information-heap/sqlplus/","noteIcon":"","created":"2026-04-02T12:17:16.827-05:00"}
---

Date: [[-Daily Activity Log-/2026 04-April 02\|2026 04-April 02]]

```cmd
sqlplus / as sysdba
```

Ovation oracle database interrogation script, on drop 200
```
-- Ovation Database Schema Interrogation
-- Target: PTADMIN Views for BioRem Refurbishment Audit

-- start the command: sqlplus / as sysdba @C:\Users\Administrator\Desktop\interrogate_views.sql

SET ECHO OFF -- hides SQL commands, to get data only out
SET FEEDBACK OFF -- suppresses summary message regarding selection, etc
SET TRIMSPOOL ON -- Removes trailing whitespace at the end of each line.
SET LINESIZE 32767 -- Sets the maximum number of characters allowed on a single line before Oracle forces a line break
SET PAGESIZE 0 -- Disables the internal pagination
SET WRAP OFF -- will truncate rather than wrap if we hit the max line size
SET VERIFY OFF -- prevent feedback when variables are reset
SET TERMOUT ON -- see the script working

SPOOL C:\Users\Administrator\Desktop\PTADMIN_Full_View_Audit.txt

PROMPT =========================================================
PROMPT STARTING PTADMIN VIEW INTERROGATION
PROMPT =========================================================

-- DDBVIEW
DEFINE view_var = 'DDBVIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- DROP_TYPE_VIEW
DEFINE view_var = 'DROP_TYPE_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- FULL_VALUE_VIEW
DEFINE view_var = 'FULL_VALUE_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- NVLXLAT_SEARCH_LOOKUP
DEFINE view_var = 'NVLXLAT_SEARCH_LOOKUP'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- OBJECT_ATTRIBUTE_VIEW
DEFINE view_var = 'OBJECT_ATTRIBUTE_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- OBJECT_HIERARCHY_EXPORT_VIEW
DEFINE view_var = 'OBJECT_HIERARCHY_EXPORT_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- OBJECT_HIERARCHY_VIEW
DEFINE view_var = 'OBJECT_HIERARCHY_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- OBJECT_TYPE_RECORD_VIEW
DEFINE view_var = 'OBJECT_TYPE_RECORD_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- SYS_DISTDB_ATTRIBUTE_VIEW
DEFINE view_var = 'SYS_DISTDB_ATTRIBUTE_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- SYS_DISTDB_HIERARCHY_VIEW
DEFINE view_var = 'SYS_DISTDB_HIERARCHY_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- SYS_DISTDB_VALUE_VIEW
DEFINE view_var = 'SYS_DISTDB_VALUE_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- SYS_LOAD_VIEW
DEFINE view_var = 'SYS_LOAD_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- SYS_OBJECT_ATTRIBUTE_VIEW
DEFINE view_var = 'SYS_OBJECT_ATTRIBUTE_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- SYS_OBJECT_VALUE_VIEW
DEFINE view_var = 'SYS_OBJECT_VALUE_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- SYS_OBJECT_XFORM_VIEW
DEFINE view_var = 'SYS_OBJECT_XFORM_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- SYS_OBJECT_XFORM_VIEW_ALL
DEFINE view_var = 'SYS_OBJECT_XFORM_VIEW_ALL'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- SYS_SID_VIEW
DEFINE view_var = 'SYS_SID_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- SYS_TYPES_ATTRIBUTE_FULL_VIEW
DEFINE view_var = 'SYS_TYPES_ATTRIBUTE_FULL_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- SYS_TYPES_ATTRIBUTE_VIEW
DEFINE view_var = 'SYS_TYPES_ATTRIBUTE_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- TYPES_ATTRIBUTE_FULL_VIEW
DEFINE view_var = 'TYPES_ATTRIBUTE_FULL_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- TYPES_ATTRIBUTE_VIEW
DEFINE view_var = 'TYPES_ATTRIBUTE_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- VALUE_SEARCH_ATTR
DEFINE view_var = 'VALUE_SEARCH_ATTR'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- VALUE_SEARCH_LOOKUP
DEFINE view_var = 'VALUE_SEARCH_LOOKUP'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

-- VALUE_VIEW
DEFINE view_var = 'VALUE_VIEW'
PROMPT >>> VIEW: &view_var
DESC PTADMIN.&view_var
SELECT * FROM PTADMIN.&view_var WHERE ROWNUM <= 20;

DESC SCADA.SCADA_HIERARCHY_VIEW
SELECT * FROM SCADA.SCADA_HIERARCHY_VIEW WHERE ROWNUM <= 20;

PROMPT =========================================================
PROMPT INTERROGATION COMPLETE
PROMPT =========================================================

SPOOL OFF
EXIT;
```


----


```sqlplus / as sysdba
SELECT owner, table_name 
FROM all_tables 
WHERE owner = 'PTADMIN' 
AND (table_name LIKE '%ATTR%' OR table_name LIKE '%TYPE%' OR table_name LIKE '%POINT%');
```

 the results are: 

OBJECT_TYPES_REF, ATTRIBUTE_REF, ATTRIBUTE_LIST_REF, TYPES_ATTRIBUTE_XREF, RECORD_TYPE_REF, SYS_OBJECT_TYPES_REF, SYS_XFORM_TYPES_REF, SYS_ATTRIBUTE_FLAGS_REF, SYS_TYPES_ATTRIBUTE_XREF, EXTPROC_ADDPOINTPARAMS, EXTPROC_POINTLIST

---

``` sqlplus
SELECT column_name 
FROM all_tab_columns 
WHERE table_name = 'ATTRIBUTE_REF' AND owner = 'PTADMIN';
```

response:
COLUMN_NAME:
ATTRIBUTE_KEY
IS_VIRTUAL
IS_KEY
VALUE_TYPE
NAME
MAX_SIZE
LABEL
HCI_TYPE
C_FLAGS
D_FLAGS
FOLDER

COLUMN_NAME:
BOX
DEFAULT_VALUE
IS_TEXTREF
IS_LIST