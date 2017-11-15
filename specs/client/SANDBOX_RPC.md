# Aragon Sandbox RPC

> **Legend**
>
> - ↔ Data is passed between the wrapper and the client.
> - → Data is passed from the client to the wrapper.
> - ← Data is passed from the wrapper to the client.

## ↔ `events`

Sets up a subscription for events on the proxy attached to the current application.

The wrapper will send events to the listener as they are caught in the event loop until the client unsubscribes.

### Request

**Parameters**: _None_

### Response

**Result**: `[event: EthereumEvent]`

## ↔ `call`

Performs a `call`, i.e. simulates a transaction without mutating state. Identical to `eth_call`.

### Request

**Parameters**: `[method: string, ...params: any]`

### Response

**Result**: `[returnValues: object]` or an error

## ↔ `intent`

Publishes an intent to the wrapper.

The wrapper will process the intent and calculate a transaction path. If no path is found (i.e. the calling entity does not have the necessary permissions) then a JSON-RPC error is sent back.

If a transaction path is found, two things can happen:

- The transaction is signed and the resulting transaction hash is sent back to the client as a response
- The transaction is rejected (i.e. not signed) and a JSON-RPC error is sent back as a response

### Request

**Parameters**: `[method: string, ...params: any]`

### Response

**Result**: `[txHash: string]` or an error

## → `notification`

Creates a notification.

A notification can optionally include an application context, which will be sent back to the application if the notification is clicked (via. the `context` RPC).

No response is sent back to the client.

### Request

**Parameters**: `[timestamp: number, body: string, context: ?any]`

### Response

_None_.

## ← `context`

Sends an application context to the client.

The interpretation of the application context is up to the client.

Application contexts can be used for shortcuts and notifications to trigger certain actions or views.

### Request

_None_.

### Response

**Result**: `[context: any]`

## ↔ `cache`

Reads or sets a key in the cache.

### Request

**Parameters**: `[mode: "get" | "set", key: string, value: ?any]`

### Response

**Result**: `[value: any]`
