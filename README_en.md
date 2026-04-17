# open-data-spaces-sdk-mock-server

ODS SDK for Onboarding - Mock server

## Overview

This project provides procedures and configuration files for running mock servers for ODS components using [Mockoon](https://mockoon.com), an open-source mock server development tool.

## Prerequisites

This repository provides mock servers for the following versions of the ODS components:

* [Identity Component](https://github.com/open-dataspaces/L3-identity-component): [v1.0.0](https://github.com/open-dataspaces/L3-identity-component/tree/v1.0.0)
* [Clearing and Payment Service](https://github.com/open-dataspaces/DCS-Payment): [v1.0.0](https://github.com/open-dataspaces/DCS-Payment/tree/v1.0.0)

## Preparation

Install the Mockoon CLI using the following command.

```bash
npm install -g @mockoon/cli
```

## Starting the Mock Servers

You can start the mock servers for each API using the following commands.

### L3 API (Port: 3001)

```bash
mockoon-cli start --data ./mockoon-l3.json
```

### Payment API (Port: 3002)

```bash
mockoon-cli start --data ./mockoon-payment.json
```

## About the Definition Files

- `mockoon-l3.json`: Generated from `apidoc/L3/api-docs.yaml`
- `mockoon-payment.json`: Generated from `apidoc/payment/openapi.json`

## Customization

You can customize Mockoon behavior by directly editing the definition files (`mockoon-*.json`) or by using the Mockoon GUI.

### 1. Changing the Port Number

By default, the L3 API is set to port `3001` and the Payment API is set to port `3002`.

#### Editing the Definition Files

Change the value of the `"port"` field at the beginning of each JSON file.

```json
"port": 3001,
```

#### Temporary Change via CLI

You can change the port at startup without modifying the definition file by specifying the `--port` option.

```bash
mockoon-cli start --data ./mockoon-l3.json --port 4000
```

### 2. Modifying Responses (Status Code / Body)

To modify the response of a specific API endpoint, edit the `"responses"` section under `"routes"` in the JSON file.

- **Status Code**: Change the value of `"statusCode"` (e.g., `200` -> `404`).
- **Response Body**: Edit the contents of the `"body"` field.
- **Response Headers**: Edit the list under `"headers"`.

### 3. Behavior Customization

- **Latency**: To simulate network latency, set a value in milliseconds in the `"latency"` field.
  
  - Global setting: `"latency"` at the JSON root level
  - Per-route setting: `"latency"` under each response
    
- **Rules**: To switch responses based on specific query parameters or headers, edit the `"rules"` field (using the GUI is recommended).

### 4. Dynamic Data Generation (Templating)

Mockoon supports templating using Handlebars.
Example: Including a request body value in the response

```json
"body": "{\"received_id\": \"{{body 'id'}}\"}"
```

For more details, refer to the [Mockoon Documentation](https://mockoon.com/docs/latest/templating/overview/).

## Using Mockoon GUI

If you use the Mockoon desktop application, you can also import and use the `mockoon-*.json` files.

## License

- This repository is provided under the MIT License.
- The copyright of the source code and related documentation belongs to NTT DATA Group Corporation and NTT DATA Corporation.

## Disclaimer

- The contents of this repository may be changed or removed without prior notice.
- The authors and maintainers assume no responsibility whatsoever for any losses or damages arising from the use of this repository.
