# net

> Wydaje żądania HTTP/HTTPS za pomocą natywnej biblioteka sieciowej Chromium

Proces: [Main](../glossary.md#main-process)

Modułu `net` jest to API po stronie klienta dla wydawania żądań HTTP(S). It is similar to the [HTTP](https://nodejs.org/api/http.html) and [HTTPS](https://nodejs.org/api/https.html) modules of Node.js but uses Chromium's native networking library instead of the Node.js implementation, offering better support for web proxies.

The following is a non-exhaustive list of why you may consider using the `net` module instead of the native Node.js modules:

* Automatic management of system proxy configuration, support of the wpad protocol and proxy pac configuration files.
* Automatic tunneling of HTTPS requests.
* Support for authenticating proxies using basic, digest, NTLM, Kerberos or negotiate authentication schemes.
* Support for traffic monitoring proxies: Fiddler-like proxies used for access control and monitoring.

The `net` module API has been specifically designed to mimic, as closely as possible, the familiar Node.js API. The API components including classes, methods, properties and event names are similar to those commonly used in Node.js.

Poniższy przykład szybko pokazuje, jak API `net` może być używane:

```javascript
const { app } = require('electron')
app.on('ready', () => {
  const { net } = require('electron')
  const request = net.request('https://github.com')
  request.on('response', (response) => {
    console.log(`STATUS: ${response.statusCode}`)
    console.log(`HEADERS: ${JSON.stringify(response.headers)}`)
    response.on('data', (chunk) => {
      console.log(`BODY: ${chunk}`)
    })
    response.on('end', () => {
      console.log('No more data in response.')
    })
  })
  request.end()
})
```

By the way, it is almost identical to how you would normally use the [HTTP](https://nodejs.org/api/http.html)/[HTTPS](https://nodejs.org/api/https.html) modules of Node.js

The `net` API can be used only after the application emits the `ready` event. Trying to use the module before the `ready` event will throw an error.

## Metody

The `net` module has the following methods:

### `net.request(options)`

* `options` (Object | String) - The `ClientRequest` constructor options.

Zwraca [`ClientRequest`](./client-request.md)

Creates a [`ClientRequest`](./client-request.md) instance using the provided `options` which are directly forwarded to the `ClientRequest` constructor. The `net.request` method would be used to issue both secure and insecure HTTP requests according to the specified protocol scheme in the `options` object.