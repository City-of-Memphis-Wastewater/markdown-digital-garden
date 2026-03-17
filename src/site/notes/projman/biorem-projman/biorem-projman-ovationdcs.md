---
{"dg-publish":true,"permalink":"/projman/biorem-projman/biorem-projman-ovationdcs/","noteIcon":"","created":"2026-03-10T08:42:35.011-05:00"}
---

Date: [[-Daily Activity Log-/2026 03-March 10\|2026 03-March 10]]

## Ovation Graphics and Points

Known graphics:
- 2000AB.diag
- 2002.diag
- 4001.diag
- PLG6000.diag
- PLG6001.diag
- PLG6002.diag
- PLG6003.diag

---

## Drawings
- 2323F-D-001
- 2323F-E-001

---

## Points

All points at the [[Vocabulary/Maxson\|Maxson]] plant have the IESS suffix ".UNIT0@NET0"
Shown below are the IDCS values, which are equivalent to the IESS values but without this suffix.

[[Information Heap/biorem-projman-drop200-database-point-analysis\|biorem-projman-drop200-database-point-analysis]]
### Points found in 4001.diag

There is a warning that 62 unresolved points exists, including: AAH4100, AAH4100-A, AAH4100-B, ...,  AAH4100-J, AIR4100-01, AIR4100-02, ..., AIR4100-66, YS4100-A, YS4100-C 

Take a backup from Drop 200.

On DROP 200, in cmd, run 

```cmd
sqlplus / as sysdba
```
### Points found in  2000ab.diag

| **IDCS**          | **Category**  | **Label**               | **Unit** |
| ----------------- | ------------- | ----------------------- | -------- |
| **B1_AIT_6015**   | Gas Detection | METHANE LEVEL           | %        |
| **B1_AIT_6020**   | Gas Detection | H2S LEVEL               | PPM      |
| **B1_PIR_5000**   | Inlet Signal  | INLET PRESSURE          | IN WC    |
| **B1_FIR_5000**   | Air Flow      | RECIRC AIR FLOW         | SCFM     |
| **PT01100**       | Air Flow      | RECIRC AIR FLOW TOTAL 1 | SCF      |
| **B1_ZIT_5700_1** | Valve         | RECIRC VALVE POSITION   | %        |
| **B1_FIR_3000**   | Waste Gas     | WASTE GAS INLET FLOW    | SCFM     |
| **B1_PIR_3000**   | Waste Gas     | WASTE GAS INLET PRESS   | IN WC    |
| **B1_TIR_5400**   | Discharge     | DISCH GAS TEMP          | °F       |
| **PT01100**       | Discharge     | DISCH GAS PRESS         | IN WC    |
| **B1_FIR_5400**   | Discharge     | DISCH GAS FLOW          | SCFM     |
| **B1_SIT_5100**   | Blower #1     | BLOWER 1 SPEED          | RPM      |
| **B1_SIT_5200**   | Blower #2     | BLOWER 2 SPEED          | RPM      |
| **B1_SIT_5300**   | Blower #3     | BLOWER 3 SPEED          | RPM      |
| **CH01088**       | System        | HEARTBEAT               | Status   |

Look in the spreadsheet for this purpose for point tracking.
## Ovation Control sheets


Known control sheets:
 It is as if the control sheets are missing.

 

```
which control sheets are these BR_ points used in? For all the points used in the biorem graphics, like the points used in pages 4001.diag, 2000ab.diag, 2002.diag, PLG6000.diag, PLG6001.diag, PLG6002.diag, and PLG6003.diag - in which various control sheets are these points used? What is the architecture of the biorem system that unifies these points and control sheets? You mentioned XML - is there some unified strucure and grouping and ornagization? it feels like these things are just painted across various graphics and control sheeets without another layer of documentation. It feels like a terrible case of "the code is the documentation"
```


---

### Relevant Ovation Tickets
- [[Ovation Help Tickets/Ovation Call NC-2600-8165\|Ovation Call NC-2600-8165]]