## Troubleshoot the error that occurs during WebSocket handshake

A *"Error during WevSocket handshake"* exception that occurs when we try to establish connection to ASP.NET Core SignalR hub can be diagnosed using the advice in this topic.

###  With unexpected response code: 400 (or 503)

The following error might be logged:

```
WebSocket connection to 'wss://xxx/HubName' failed: Error during WebSocket handshake: Unexpected response code: 400

Error: Failed to start the connection: Error: There was an error with the transport.
```

The error is usually caused by client only use WebSockets transport but WebSockets protocol is not enabled on server.

###  With unexpected response code: 307

```
WebSocket connection to 'ws://xxx/HubName' failed: Error during WebSocket handshake: Unexpected response code: 307
```

The most common cause of this issue is when SignalR hub server listen to and respond over both HTTP and HTTPS, and configured enforce HTTPS by calling `UseHttpsRedirection` in the `Startup` class, or ebforce HTTPS via URL Rewrite rule etc. 

If specifying the URL on client side using `.withUrl("http://xxx/HubName")`, which would cause this error. To fix it, can modify the code to use HTTPS URL or not enforce HTTPS.

###  With unexpected response code: 404

```
WebSocket connection to 'wss://xxx/HubName' failed: Error during WebSocket handshake: Unexpected response code: 404
```

If application could work as expected on local, but got this error after published to IIS server etc. Please check if the ASP.NET Core SignalR app is hosted as an IIS sub-application, but do not set URL with the sub-app's pathbase on SignalR JavaScript client side `.withUrl("/SubAppName/HubName")`.
