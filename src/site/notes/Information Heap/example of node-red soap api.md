---
{"dg-publish":true,"permalink":"/information-heap/example-of-node-red-soap-api/","noteIcon":"","created":"2025-12-02T21:21:50.989-06:00"}
---

Date: [[-Daily Activity Log-/2025 12-December 02\|2025 12-December 02]]

That's excellent news! Installing `pce-node-red-contrib-soap-plus` (the most recent fork) is the key first step.

Now, let's implement the **Login and Session Management** part of your flow, as it replaces the critical authentication logic found at the start of your Python client (`soapclient.service.login(...)`).

The goal is to move from the existing `http request` flow (which we'll remove/ignore) to the structured SOAP flow.

### 📝 Edited Flow Components: Login and Session

Here are the four nodes for the initial setup, login, and session persistence.

#### 1. Node A: Inject (Trigger and Arguments)

- **Type:** `inject`
    
- **Purpose:** Triggers the flow and passes in the dynamic inputs (the base IESS identifiers and the required suffix, which are analogous to your `idcs` list and `idcs_to_iess_suffix` configuration).
    
- **Payload:** Set to **JSON**
    
- **Value:**
    

JSON

```
{
    "base_idcs": ["I-0300A", "I-0300B", "F-1000C"],
    "iess_suffix": ".UNIT1@NET1",
    "plant_name": "WWTF"
}
```

#### 2. Node B: Change (IESS List Preparation)

- **Type:** `change`
    
- **Purpose:** Builds the final list of IESS names, removing the hard-coded value found in your Python client's demo section (`'I-0300A.UNIT1@NET1'`).
    
- **Rule 1: Set `msg.iess_list`**
    
    - **Set:** `msg.iess_list`
        
    - **To:** **JSONata** Expression:
        
        Code snippet
        
        ```
        payload.base_idcs ~> $map(function($v) {
            $v & payload.iess_suffix
        })
        ```
        
    
    _(This takes the `base_idcs` array, and concatenates each element with the `iess_suffix`, creating a list like `["I-0300A.UNIT1@NET1", "I-0300B.UNIT1@NET1", ...]`)_
    
- **Rule 2: Set WSDL URL (Simulation of `get_eds_soap_api_url`)**
    
    - **Set:** `msg.url`
        
    - **To:** **String** (Replace with your actual WSDL URL, e.g., `http://10.0.0.1:43080/eds.wsdl`)
        
- **Rule 3: Set Credentials (Simulation of `get_eds_soap_api_credentials`)**
    
    - **Set:** `flow.username`
        
    - **To:** **String** (Your SOAP Username)
        
    - **Set:** `flow.password`
        
    - To: String (Your SOAP Password)
        
        (Note: We use flow context for username/password as they are static settings needed across nodes, and we only pass the necessary inputs in msg)
        

#### 3. Node D: SOAP Login

- **Type:** `soap` (from the `pce-node-red-contrib-soap-plus` palette)
    
- **Name:** `SOAP Login`
    
- **Client Configuration (New):**
    
    - **WSDL URL:** Leave blank (we will use `msg.url`)
        
    - **Authentication:** Select **Basic Auth**
        
    - **Username:** Leave blank (we will use `msg.username`)
        
    - **Password:** Leave blank (we will use `msg.password`)
        
- **Message Configuration:**
    
    - **WSDL URL:** `msg.url`
        
    - **Service:** (Lookup the WSDL service name, e.g., `EdsService`)
        
    - **Operation:** `login`
        
    - **Input Message:** Set to **Expression**. The `login` service takes two arguments: `username` and `password`.
        
        Code snippet
        
        ```
        {
            "username": $flowContext('username'),
            "password": $flowContext('password')
        }
        ```
        
- **Note:** We use a function node _before_ this (Node C) to check if we need to log in, but for simplicity, we can connect the Change node (B) directly to the SOAP node (D) for now, and handle the logic in Node E.
    

#### 4. Node E: Function (Store `authstring`)

- **Type:** `function`
    
- **Name:** `Store Authstring`
    
- **Purpose:** Captures the response from the `login` call and saves the session ID (`authstring`) into the flow context for subsequent API calls.
    
- **Code:**
    

JavaScript

```
// The SOAP-Plus node output is in msg.payload. The result of the 'login'
// operation should be the authstring itself, or contained within a response object.
// We assume it returns the string directly, which is common.

let authstring = msg.payload;

// Check if login failed (e.g., payload is null or an empty string)
if (!authstring || typeof authstring !== 'string' || authstring.length < 10) {
    node.error("SOAP Login failed or returned invalid authstring.", msg);
    return null; // Stop the flow
}

// Store the valid authstring in flow context, similar to your Python client's state.
flow.set('authstring', authstring);
node.status({fill:"green", shape:"dot", text:"Login Success"});

node.warn(`Login successful. Authstring: ${authstring.substring(0, 10)}...`);

// The flow is now authenticated. Pass the original message (with iess_list)
// to the next stage (the RequestTabular flow).
return msg;
```

---

### 💾 Node-RED Flow JSON

Here is the JSON for the **Flow 1** tab, incorporating the new nodes and removing the obsolete `http request` chain. You can import this into your Node-RED editor (Import -> Paste the full JSON).

JSON

```
[
    {
        "id": "e35712358b6bc22b",
        "type": "tab",
        "label": "EDS SOAP Client Flow",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "1e72b4f910a30b42",
        "type": "inject",
        "z": "e35712358b6bc22b",
        "name": "Start Flow / Arguments",
        "props": [
            {
                "p": "payload"
            },
            {
                "p": "topic",
                "v": "",
                "vt": "string"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "{\"base_idcs\": [\"I-0300A\", \"I-0300B\", \"F-1000C\"], \"iess_suffix\": \".UNIT1@NET1\", \"plant_name\": \"WWTF\"}",
        "payloadType": "json",
        "x": 150,
        "y": 140,
        "wires": [
            [
                "60f6448a614742c3"
            ]
        ]
    },
    {
        "id": "60f6448a614742c3",
        "type": "change",
        "z": "e35712358b6bc22b",
        "name": "Prep IESS List & Config",
        "rules": [
            {
                "t": "set",
                "p": "iess_list",
                "pt": "msg",
                "to": "payload.base_idcs ~> $map(function($v) { $v & payload.iess_suffix })",
                "tot": "jsonata"
            },
            {
                "t": "set",
                "p": "url",
                "pt": "msg",
                "to": "http://10.0.0.1:43080/eds.wsdl",
                "tot": "str"
            },
            {
                "t": "set",
                "p": "username",
                "pt": "flow",
                "to": "admin_user",
                "tot": "str"
            },
            {
                "t": "set",
                "p": "password",
                "pt": "flow",
                "to": "admin_password_123",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 370,
        "y": 140,
        "wires": [
            [
                "e31f29a0082c5f59"
            ]
        ]
    },
    {
        "id": "e31f29a0082c5f59",
        "type": "soap",
        "z": "e35712358b6bc22b",
        "name": "SOAP Login",
        "wsdl": "",
        "service": "",
        "method": "login",
        "headers": {},
        "action": "login",
        "timeout": "10000",
        "url": "{{url}}",
        "input": "{\"username\": $flowContext('username'),\"password\": $flowContext('password')}",
        "inputType": "jsonata",
        "soapAction": "",
        "auth": true,
        "username": "{{username}}",
        "password": "{{password}}",
        "x": 580,
        "y": 140,
        "wires": [
            [
                "c0d1646274e14f09"
            ]
        ]
    },
    {
        "id": "c0d1646274e14f09",
        "type": "function",
        "z": "e35712358b6bc22b",
        "name": "Store Authstring",
        "func": "let authstring = msg.payload;\n\n// Check if login failed (e.g., payload is null or an empty string)\nif (!authstring || typeof authstring !== 'string' || authstring.length < 10) {\n    node.error(\"SOAP Login failed or returned invalid authstring.\", msg);\n    return null; // Stop the flow\n}\n\n// Store the valid authstring in flow context\nflow.set('authstring', authstring);\nnode.status({fill:\"green\", shape:\"dot\", text:\"Login Success: \" + authstring.substring(0, 10) + \"...\"});\n\n// Pass the message to the next stage (e.g., the tabular request logic)\nreturn msg;",
        "outputs": 1,
        "timeout": 0,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 780,
        "y": 140,
        "wires": [
            [
                "89a9c2b8c9d96c4d"
            ]
        ]
    },
    {
        "id": "89a9c2b8c9d96c4d",
        "type": "debug",
        "z": "e35712358b6bc22b",
        "name": "Debug: Authenticated!",
        "active": true,
        "tosidebar": true,
        "console": false,
        "tostatus": false,
        "complete": "payload",
        "targetType": "msg",
        "statusVal": "",
        "statusType": "auto",
        "x": 1000,
        "y": 140,
        "wires": []
    }
]
```

### Next Step: Tabular Data Request (Core Logic)

Now that you are authenticated and have the `authstring` and `iess_list` in context/message, the next step is to initiate the asynchronous tabular data request, which replaces **Example 6** in your Python script:

Python

```
# 8. Send the request
request_id = soapclient.service.requestTabular(authstring, tab_request)
```

We will need a **Function Node** to construct the complex `TabularRequest` object required by your EDS API, as the `soap-plus` node expects the SOAP payload structure.

**❓ Are you ready to move on to the next set of nodes: preparing the request object and calling `requestTabular`?**