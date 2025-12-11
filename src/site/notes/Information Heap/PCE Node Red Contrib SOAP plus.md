---
{"dg-publish":true,"permalink":"/information-heap/pce-node-red-contrib-soap-plus/","noteIcon":"","created":"2025-12-02T20:48:45.522-06:00"}
---

Date: [[-Daily Activity Log-/2025 12-December 02\|2025 12-December 02]]

https://flows.nodered.org/node/pce-node-red-contrib-soap-plus
https://www.npmjs.com/package/pce-node-red-contrib-soap-plus

[[Information Heap/example of node-red soap api\|example of node-red soap api]]
# pce-node-red-contrib-soap-plus 1.0.0

SOAP Client for Node Red with Basic Authentication when retrieving the WSDL.

`npm install pce-node-red-contrib-soap-plus`[](https://nodered.org/docs/getting-started/adding-nodes.html)

# node-red-contrib-soap-plus

Node-red nodes that allow users to send SOAP request.

This is a fork from node-red-contrib-soap to publish fixes on NPM The original project and its authors deserve full credit for the initial implementation.

##Usages To set up SOAP request, create a new soap config:

- Enter the WSDL address to the `WSDL` field.
- Select `Auth` method, the default would be set to None.
- Fill in _Username_, _Password_, _Client Key_, _Client Certificate_, _Token_ if needed.

In the SOAP request config tab, enter the valid method name in _Method_ field. Having an invalid method name will return error.

You can also feed in parameters and overwrite configs with an injected msg:

- You can have `msg.server` to overwrite the WSDL address. This only works with WSDL server with no authentication method.
- You can have `msg.options` to add in options to the SOAP request.
- You can have `msg.headers` to add in headers for the SOAP request.
- You can have `msg.headerFunction` (or `msg.headers` as a function) to dynamically generate SOAP headers.
- You can feed in `msg.payload.<parameters>` to feed in the parameters you need.

For example, here is the sample flow where we try to send a SOAP request to [http://www.webservicex.net/geoipservice.asmx](http://www.webservicex.net/geoipservice.asmx), trying to call the `GetGeoIP` function with an variable IPAddress as 139.130.4.5.

```
[{"id":"e480824b.767f","type":"soap request","z":"beb01f75.a7c1d","name":"","topic":"","wsdl":"1d0b9627.18e18a","method":"GetGeoIP","x":395,"y":169,"wires":[["5bd47eca.8fd67"]]},{"id":"a2176fcf.cb88","type":"inject","z":"beb01f75.a7c1d","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":114,"y":261,"wires":[["5e12abf.dccd454"]]},{"id":"5bd47eca.8fd67","type":"debug","z":"beb01f75.a7c1d","name":"","active":true,"console":"false","complete":"false","x":541,"y":265,"wires":[]},{"id":"5e12abf.dccd454","type":"function","z":"beb01f75.a7c1d","name":"","func":"var newmsg={\n  server:\"http://www.webservicex.net/geoipservice.asmx\",\n  options:{},\n  headers:{},\n  payload:{\n      IPAddress:\"139.130.4.5\"\n  }\n  \n};\nreturn newmsg;","outputs":1,"noerr":0,"x":246,"y":194,"wires":[["e480824b.767f"]]},{"id":"1d0b9627.18e18a","type":"soap-config-plus","z":"beb01f75.a7c1d","wsdl":"http://www.webservicex.net/geoipservice.asmx?wsdl","auth":"0","user":"","pass":"","key":"","cert":"","token":""}]
```

The returned `msg.payload` would be:

```
{ "GetGeoIPResult": { "ReturnCode": 1, "IP": "139.130.4.5", "ReturnCodeDetails": "Success", "CountryName": "Australia", "CountryCode": "AUS" } }
```


> |#### Node Info
> 
> Version: 1.0.0
> 
> Updated 1 week, 5 days ago
> 
> License: MIT
> 
> Rating: 5.0 1
> 
> [View on npm](https://npmjs.org/package/pce-node-red-contrib-soap-plus)
> 
> [View Scorecard 6  6](https://flows.nodered.org/node/pce-node-red-contrib-soap-plus/scorecard)
> 
> #### Categories
> 
> #### Actions
> 
> #### Downloads
> 
> 0 in the last week
> 
> #### Nodes
> 
> - soap request plus
>     
> - soap-config-plus
>     
> 
> #### Keywords
> 
> - soap
>  - node-red
>  - node
> 
> #### Maintainers
> 
> - [pce.noliver](https://flows.nodered.org/user/pce.noliver)
> 
> #### Contributors
> 
> - Andre Alexandre Avila
> - Michael Qiu
> - Andre Guerra
> - Nico Oliver     


https://www.npmjs.com/package/pce-node-red-contrib-soap-plus

