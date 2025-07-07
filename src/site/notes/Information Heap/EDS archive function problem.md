---
{"dg-publish":true,"permalink":"/information-heap/eds-archive-function-problem/","noteIcon":"","created":"2025-07-07T14:23:45.378-05:00"}
---

The EDS_AV() function works to provide real-time access to a given point. It is only the EDS archive functions, which access one of more time values, that do not work.



Email  "EDS Archive functions" , on [[-Daily Activity Log-/2025 01-January 30\|2025 01-January 30]]
- Hi Geovanny,
	Here at City of Memphis are having a hard time referencing time values using the EDS Excel plugin, specifically with any of the "EDS archives" functions. For example, the EDS_Avg() function does not work as expected, as shown in the EDS Excel Plug-in User Guide (EDS920_50, Version 9.2.0, February 2018) and on page 19 (page 23 of the PDF). 
	
	I would like to use the EDS_Value() function, which references two inputs: the point name as well as a time value. But, when I use it, the results come back as #NULL!	

	Do you know what we can do to get these functions working? This way we can explicitly reference times as desired, rather than simply using the Tabular Trend pop-up window.
	
	There is a difference between a #NULL! error and a #NAME! error. I suspect that I am using the EDS_Value() function as intended because I am getting a #NULL! error, whereas I get a #NAME! error when I purposefully use erroneous inputs.