# Aragon Background Tasks

This spec defines how the Aragon client should handle application background tasks.

## Motivation

Applications should be able to synchronise state and send notifications even if the application is not open.

## Defining background tasks

Applications are already shipped with a standard web manifest (`manifest.json`), and the specification for web manifests also specify how to run background tasks:

>Use the background key to include one or more background scripts, and optionally a background page in your extension.
>
>Background scripts are the place to put code that needs to maintain long-term state, or perform long-term operations, independently of the lifetime of any particular web pages or browser windows.
>
> [...]
>
> (The `scripts` key is) an array of strings, each of which is a path to a JavaScript source. The path is relative to the manifest.json file itself. These are the background scripts that will be included in the extension.
>
> – [Mozilla Developer Network](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/manifest.json/background)

## Executing background tasks

Background tasks for Aragon applications work in exactly the same way, with two modifications: background pages are ignored, and only the first background script is loaded.

## Flow

1. The application is registered in the wrapper by `@aragon/wrapper`
2. The wrapper should check if the `manifest.json` defines any background workers
3. If any background workers are defined, the wrapper should execute the defined script in a web worker.

## Best Practices

Background workers should ideally handle event logs and reduce them to an application state. Furthermore, web workers should send notifications in response to events, if necessary.

The front-end portion of Aragon applications should only read the computed state from the web worker, and should not compute state themselves.
