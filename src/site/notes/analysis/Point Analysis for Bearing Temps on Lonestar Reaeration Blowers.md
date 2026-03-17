---
{"dg-publish":true,"permalink":"/analysis/point-analysis-for-bearing-temps-on-lonestar-reaeration-blowers/","noteIcon":"","created":"2026-03-17T14:06:21.555-05:00"}
---

Date: [[-Daily Activity Log-/2026 03-March 17\|2026 03-March 17]]

## Purpose
We are inspecting whether or not bearing temperatures were effectively monitored for the  the [[Lonestar\|Lonestar]] [[Reaeration Blowers\|Reaeration Blowers]] 

#reaeration #lonestar #blowers

---

## Blower 1, B-165
These points are found on control sheet: "RAS BLOWER 1 (B-165) STEERING", which is on Drop 6, Task 2, Sheet 191

| IDCS     | Description                |
| -------- | -------------------------- |
| TAH1654  | B-165 BRNG TMP HI SPD FRNT |
| TAH1655  | B-165 BRNG TMP HI SPD REAR |
| TAH1656  | B-165 BRNG TMP LO SPD FRNT |
| TAH1657  | B-165 BRNG TMP LO SPD REAR |
| TAH1653  | B-165 HI MTR TMP COIL U    |
| TAH1659  | B-165 HI MTR TMP COIL V    |
| TAH16510 | B-165 HI MTR TMP COIL W    |

All of these show 0 and "Normal" for the past year, from the EDS and Trend.

--

```bash
eds trend tah1654 tah1655 tah1656 tah1657 tah1653 tah1659 tah16510 --days 365 -dp 4000
```

![Pasted image 20260317141234.png](/img/user/Pasted%20image%2020260317141234.png)

---

## Blower 1, B-166
These points are found on control sheet: "RAS BLOWER 2 (B-166) STEERING", which is on Drop 6, Task 2, Sheet 193

| IDCS     | Description                |
| -------- | -------------------------- |
| TAH1664  | B-166 BRNG TMP HI SPD FRNT |
| TAH1665  | B-166 BRNG TMP HI SPD REAR |
| TAH1666  | B-166 BRNG TMP LO SPD FRNT |
| TAH1667  | B-166 BRNG TMP LO SPD REAR |
| TAH1663  | B-166 HI MTR TMP COIL U    |
| TAH1669  | B-166 HI MTR TMP COIL V    |
| TAH16610 | B-166 HI MTR TMP COIL W    |

--

All of these show 0 and "Normal" for the past year, from the EDS and Trend.

```bash
eds trend tah1664 tah1665 tah1666 tah1667 tah1663 tah1669 tah16610 --days 365 -dp 4000
```

![Pasted image 20260317141820.png](/img/user/Pasted%20image%2020260317141820.png)

---

## Blower 1, B-167
These points are found on control sheet: "RAS BLOWER 3 (B-167) STEERING", which is on Drop 6, Task 2, Sheet 195

| IDCS     | Description                |
| -------- | -------------------------- |
| TAH1674  | B-167 BRNG TMP HI SPD FRNT |
| TAH1675  | B-167 BRNG TMP HI SPD REAR |
| TAH1676  | B-167 BRNG TMP LO SPD FRNT |
| TAH1677  | B-167 BRNG TMP LO SPD REAR |
| TAH1673  | B-167 HI MTR TMP COIL U    |
| TAH1679  | B-167 HI MTR TMP COIL V    |
| TAH16710 | B-167 HI MTR TMP COIL W    |

--

Most of these show 0 and "Normal" for the past year, from the EDS and Trend. All four of the bearing temp sensors for Blower 3 (TAH1674, TAH1675, TAH1676, TAH1677) showed a high value of 1.0 on Nov 21, 2025, for eight minutes, from 10:33 AM to 10:41 AM.

```bash
eds trend tah1674 tah1675 tah1676 tah1677 tah1673 tah1679 tah16710 --days 365 -dp 4000
```

![Pasted image 20260317142014.png](/img/user/Pasted%20image%2020260317142014.png)

![Pasted image 20260317143031.png](/img/user/Pasted%20image%2020260317143031.png)

![Pasted image 20260317143709.png](/img/user/Pasted%20image%2020260317143709.png)

[[reaeration-blowers-tah1677_21Nov2025.html]]

---

## Ovation Alarm App:

Search all historic alarms for these points. None found for Blower 2, only for Blower 3, on Nov 21, 2025.

![alarm-TAH1674,1675,1676,1677-21November2025.png](/img/user/alarm-TAH1674,1675,1676,1677-21November2025.png)