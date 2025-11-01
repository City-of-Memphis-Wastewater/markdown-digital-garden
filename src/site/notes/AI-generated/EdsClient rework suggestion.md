---
{"dg-publish":true,"permalink":"/ai-generated/eds-client-rework-suggestion/","noteIcon":"","created":"2025-10-31T23:37:51.681-05:00"}
---

Date: [[-Daily Activity Log-/2025 10-October 31\|2025 10-October 31]]

```
import requests

import time

import sys

import logging

# Mocked imports for clarity, assume these are defined elsewhere

class SecurityAndConfig:

    @staticmethod

    def get_credential_with_prompt(service, item, prompt, hide=False, overwrite=False): return "mock_value"

    @staticmethod

    def get_config_with_prompt(name, prompt): return "mock_value"

class SudsClient:

    def __init__(self, wsdl_url): print(f"SOAP Client initialized for {wsdl_url}")

    class service:

        @staticmethod

        def login(u, p): return f"AUTHSTRING-{int(time.time())}"

        @staticmethod

        def logout(authstring): return "Logged out"

        @staticmethod

        def ping(authstring): return "Pong"

class Redundancy:

    @staticmethod

    def return_hint(recipient=None, attribute_name=None): return lambda func: func

def get_base_url_config_with_prompt(service_name, prompt_message): return "http://example.com"

def get_configurable_default_plant_name(): return "Stiles"

def log_function_call(level): return lambda func: func

  
  

class EdsClient:

    """

    EdsClient handles login and data retrieval for the EDS API, supporting

    both REST (session-based) and SOAP (authstring-based) methods.

    """

    # Store configuration templates as class attributes for easy reference

    DEFAULT_SOAP_PORT = 43080

    DEFAULT_SOAP_SUB_PATH = 'eds.wsdl'

  

    def __init__(self,

                 plant_name: str,

                 rest_session: requests.Session,

                 soap_authstring: str | None,

                 soap_url: str):

        """Initializes the client with authenticated components."""

        self.plant_name = plant_name

        self.session = rest_session     # For REST API calls

        self.authstring = soap_authstring # For SOAP API calls

        self.soap_url = soap_url        # WSDL URL for potential re-initialization

        self.soapclient = None          # SOAP client instance

    # --- Context Management (Pattern 2) ---

    def __enter__(self):

        """Called upon entering the 'with' block."""

        return self

  

    def __exit__(self, exc_type, exc_val, exc_tb):

        """Called upon exiting the 'with' block (for cleanup)."""

        # 1. Close REST Session

        if hasattr(self, "session"):

            print(f"[{self.plant_name}] Closing REST session.")

            self.session.close()

        # 2. Logout from SOAP (if login was performed)

        if self.authstring:

            print(f"[{self.plant_name}] Attempting SOAP logout...")

            try:

                # We need a SOAP client instance to perform the logout

                if self.soapclient is None:

                    # Initialize just to logout, if not done already

                    self.soapclient = SudsClient(self.soap_url)

                self.soapclient.service.logout(self.authstring)

                print(f"[{self.plant_name}] Logout successful.")

            except Exception as e:

                print(f"[{self.plant_name}] Error during SOAP logout: {e}")

        # Return False to propagate exceptions, or True to suppress them

        return False

    # --- Client Creation/Login (Pattern 1 & 3) ---

    @classmethod

    def login_for_plant(cls, plant_name: str | None = None) -> "EdsClient":

        """

        Performs all necessary setup, authentication, and returns an EdsClient instance.

        """

        if plant_name is None:

             plant_name = get_configurable_default_plant_name()

        # --- Credential and Configuration Retrieval (Moved out of data methods) ---

        service_name = EdsClient.get_service_name(plant_name)

        base_url = get_base_url_config_with_prompt(

            service_name=f"{plant_name}_eds_base_url",

            prompt_message=f"Enter {plant_name} EDS base url"

        )

        username = SecurityAndConfig.get_credential_with_prompt(service_name, "username", f"Enter EDS API username for {plant_name}", hide=False)

        password = SecurityAndConfig.get_credential_with_prompt(service_name, "password", f"Enter EDS API password for {plant_name}")

        # --- Authentication ---

        # 1. REST Login

        rest_api_url = cls.get_rest_api_url(base_url=base_url)

        rest_session = cls._perform_rest_login(rest_api_url, username, password)

        # 2. SOAP Setup and Login (Moved from soap_api_iess_request_single)

        soap_url = cls.get_soap_api_url(base_url=base_url)

        soapclient = SudsClient(soap_url)

        soap_authstring = soapclient.service.login(username, password)

        # 3. Create and Return Client Instance

        client = cls(

            plant_name=plant_name,

            rest_session=rest_session,

            soap_authstring=soap_authstring,

            soap_url=soap_url

        )

        # Store the initialized soapclient on the instance if needed for subsequent calls

        client.soapclient = soapclient

        print(f"[{plant_name}] Client successfully created and authenticated.")

        return client

  

    # Helper method for REST login (from user's original login_to_session)

    @staticmethod

    def _perform_rest_login(api_url: str, username: str, password: str, timeout: int = 10):

        """Performs the REST login and returns an authenticated requests.Session."""

        session = requests.Session()

        data = {'username': username, 'password': password, 'type': 'script'}

        response = session.post(f"{api_url}/login", json=data, verify=False, timeout=timeout)

        response.raise_for_status()

        json_response = response.json()

        session.headers['Authorization'] = f"Bearer {json_response['sessionId']}"

        return session

  

    # --- Data Retrieval (Now an instance method using self.authstring) ---

    def soap_api_iess_request_single(self, idcs: list[str] | None):

        """

        Refactored: Now uses the authstring and soapclient pre-loaded on the instance.

        The complex setup logic is gone!

        """

        if not self.authstring:

            print("Error: Client not logged in for SOAP.")

            return

  

        print(f"\n--- Instance: {self.plant_name} ---")

        print(f"Using authstring: {self.authstring[:10]}...")

        # Use the pre-initialized SOAP client

        soapclient = self.soapclient

        # Example 1: ping

        print("Pinging server...")

        soapclient.service.ping(self.authstring)

        print("Ping successful.")

        # The rest of your data logic (getServerTime, getPoints, etc.) goes here,

        # using self.authstring and self.soapclient

        # Example 4: Get a specific point by IESS name

        # ... logic ...

        # NOTE: Logout is now handled automatically by __exit__

        pass

  

    # --- Configuration Builders (Static/Class Methods) ---

    @staticmethod

    def get_service_name(plant_name: str) -> str:

        return f"{plant_name}_eds_api"

    @classmethod

    @Redundancy.return_hint(recipient="cls"|None,attribute_name="soap_api_url")

    def get_soap_api_url(cls, base_url: str | None = None, eds_soap_api_port: int | None = DEFAULT_SOAP_PORT, eds_soap_api_sub_path: str | None = DEFAULT_SOAP_SUB_PATH) -> str | None:

        """Recipe for forming the SOAP API WSDL URL."""

        if not base_url or not str(eds_soap_api_port) or not eds_soap_api_sub_path:

            return None

        return f"{base_url}:{eds_soap_api_port}/{eds_soap_api_sub_path}"

  

    @classmethod

    def get_rest_api_url(cls, base_url: str | None = None, eds_rest_api_port: int | None = DEFAULT_SOAP_PORT, eds_rest_api_sub_path: str = 'api/v1') -> str | None:

        """Recipe for forming the REST API URL."""

        if not base_url or not str(eds_rest_api_port):

             return None

        return f"{base_url}:{eds_rest_api_port}/{eds_rest_api_sub_path}"

  

# --- Example Usage Demonstration ---

@log_function_call(level=logging.DEBUG)

def demo_eds_refactor_usage():

    print("--- Demo: Using the Refactored EdsClient with Context Manager ---")

    # Use the context manager (Pattern 2)

    # The login process (Pattern 1 & 3) handles credential retrieval, REST login,

    # and SOAP login, storing the session and authstring on the client instance.

    try:

        with EdsClient.login_for_plant(plant_name="Stiles") as stiles_client:

            print("\nSuccessfully logged in and entered context manager.")

            stiles_client.soap_api_iess_request_single(idcs=['I-0300A'])

            # This line will run before __exit__ is called

            print("\nExiting context manager: __exit__ will automatically logout and close the REST session.")

    except Exception as e:

        print(f"\nAn error occurred during execution: {e}")

if __name__ == "__main__":

    demo_eds_refactor_usage()
```

---

That's a fantastic observation! The `MissionClient` uses several robust patterns that make it cleaner and safer to use, especially for handling external APIs that require tokens or session management.

Here is a breakdown of the key patterns to borrow for your `EdsClient`, followed by a proposed refactoring file.

---

## **Refactoring Patterns for `EdsClient`**

The core principle is **"Client Instantiation = Successful Authentication."**

### 1. Unified Client Creation (`@classmethod` Login)

- **Before:** Your current `@staticmethod EdsClient.login_to_session` returns a raw `requests.Session`. This forces the calling code to manually manage the session and create the `EdsClient` instance later.
    
- **Borrow:** `MissionClient.login_to_session` is a **`@staticmethod` (or `@classmethod`)** that handles authentication _and_ returns a fully initialized `MissionClient` instance that already holds the authenticated session (`self.session`).
    

### 2. Context Management (`__enter__` and `__exit__`)

- **Before:** You manually handle `try/finally` for `soapclient.service.logout(authstring)` in `soap_api_iess_request_single`.
    
- **Borrow:** Implement `__enter__` and `__exit__`. The `__exit__` method automatically handles:
    
    - Closing the REST `requests.Session`.
        
    - Performing the SOAP `logout(authstring)` call, guaranteeing cleanup even if an exception occurs during data retrieval. This allows usage with `with EdsClient.login(...) as client: ...`.
        

### 3. Instance-Bound Credentials and Session

- **Before:** Your data methods (`soap_api_iess_request_single`) are static, meaning they have to repeatedly retrieve credentials (`username`, `password`), configuration (`base_url`), and perform the login (`soapclient.service.login`) every time they are called.
    
- **Borrow:** Once a client instance is created via `EdsClient.login()`, the authenticated REST session (`self.session`) and the SOAP `authstring` (`self.authstring`) should be stored on that instance in the `__init__`. All subsequent data methods then just use `self.session` or `self.authstring`. This makes the data methods cleaner, faster, and simpler.
    

---

I've refactored your `EdsClient` structure to incorporate these practices, making it ready for both REST and SOAP calls after a single login process.

I will introduce the refactoring in a new file, as your existing snippet requires several external definitions.

Refactored EdsClient Proposal

Oct 31, 11:36 PM

This refactoring centralizes all your authentication logic into the `EdsClient.login_for_plant()` class method (Pattern 1). This ensures that once you have an `EdsClient` instance, all the necessary authenticated components (`self.session`, `self.authstring`) are available for your instance methods (like the simplified `soap_api_iess_request_single`), which is Pattern 3. Crucially, the use of `__exit__` guarantees cleanup (logout/close) every time (Pattern 2).