---
{"dg-publish":true,"permalink":"/information-heap/data-streaming-in-and-out-of-ovation/","noteIcon":"","created":"2025-05-20T09:18:16.301-05:00"}
---

Date: [[-Daily Activity Log-/2025 03-March 12\|2025 03-March 12]]

Pete:

> Clayton,
> 
> You mentioned that there is a request is to feed data originating in EDS into Clarity. Are you referring to Microsoft Clarity or Clarity the workflow management software? That will help to determine what data connectors are supported. If Microsoft Clarity, that is typically for monitoring web traffic so perhaps look at Microsoft fabric. I have a little bit of experience with Clarity on the workflow management side for manufacturing.
> 
> For the EDS portion, extract data from the EDS server to display on business systems, like Power BI, there are a couple routes:
> 
> 1. Link to an excel spreadsheet with the EDS add-in. As you mentioned on the phone this is not streamlined and not a good long term solution. Also, data cannot be written to EDS this way.
> 2. EDS supports a communication protocol call OPC. This is an open protocol designed to transmit time series data meaning that you can access the most recent values of all of the points. It does not allow you to look at historical data. Microsoft does not support OPC directly so an SQL server would need to be built to ingest OPC data. Then the Microsoft ecosystem could read data from the SQL server. We could use this to write data from any data source back into EDS this way. Hach Wims will have a SQL server we could use long term.
> 3. Like item 2 above. The EDS server supports a communication protocol call Modbus. This is the same type of protocol that we were using to pull PAA data from the Evonik PLC’s in Ovation. Just like OPC above, you’d need a SQL server for Microsoft products to access.
> 4. EDS supports writing values to a file. You could set it up to have values writing to a .txt file that clarity could access. You may be limited on how often you can write to the file so if high resolution data is desired, this may not be the best route.
> 5. EDS has a python API that could be used. We could put together some code to convert EDS data to whatever format you like via python and would certainly be the most flexible, but I’m always wary of the long-term maintainability of custom coded solutions. If we all happen to win the lottery, our replacements may have a hard time supporting it.
> 6. EDS uses a SQL server which I think we could access directly, but there is no documentation explaining this connection. They also allude to a Rest API, but again I cannot find any details on it. We could explore with Emerson if you would like.
> 
> Attached are some relevant documents. The data interfaces manual is most relevant.
> 
> Please call or respond with any questions or if you would like to discuss in more detail.
> 
> **Peter Gabor**
> 