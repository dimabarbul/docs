---
description: "Learn more about: Extended Protection Policy"
title: "Extended Protection Policy"
ms.date: "03/30/2017"
ms.assetid: e2616a10-317e-4c34-8023-0c015a80a82f
---
# Extended Protection Policy

This article describes the ExtendedProtection sample.

Extended Protection is a security initiative for protecting against man-in-the-middle (MITM) attacks. A MITM attack is a security threat in which a MITM takes a client's credentials and forwards it to a server.

## Discussion

When applications authenticate using Kerberos, Digest or NTLM using HTTPS, a Transport Level Security (TLS) channel is first established and then authentication takes place using the secure channel. However, there is no binding between the session key generated by SSL and the session key generated during authentication. Any MITM can establish itself between the client and the server and start forwarding the requests from the client, even when the transport channel itself is secure, because the server has no way of knowing whether the secure channel has been established from the client or a MITM. The solution in this scenario is to bind the outer TLS channel with the inner authentication channel such that the server can detect if there is a MITM.

> [!NOTE]
> This sample only works when hosted on IIS and cannot work on Cassini – Visual Studio Development Server because Cassini does not support HTTPS.

> [!NOTE]
> This feature is currently only available on Windows 7. The following steps are specific to Windows 7.

#### To set up, build, and run the sample

1. Install Internet Information Services from **Control Panel**, **Add/Remove Programs**, **Windows Features**.

2. Install **Windows Authentication** in **Windows Features**, **Internet Information Services**, **World Wide Web Services**, **Security**, and **Windows Authentication**.

3. Install **Windows Communication Foundation HTTP Activation** in **Windows Features**, **Microsoft .NET Framework 3.5.1**, and **Windows Communication Foundation HTTP Activation**.

4. This sample requires the client to establish a secure channel with the server, so it requires the presence of a server certificate which can be installed from Internet Information Services (IIS) Manager.

    1. Open IIS Manager. Open **Server certificates**, which appears in the **Feature View** tab when the root node (machine name) is selected.

    2. For the purpose of testing this sample, create a self-signed certificate. If you do not want Internet Explorer to prompt you about the certificate not being secure, install the certificate in the Trusted Certificate Root authority store.

5. Open the **Actions** pane for the default Web site. Click **Edit Site**, **Bindings**. Add HTTPS as a type if not already present, with port number 443. Assign the SSL certificate created in the preceding step.

6. Build the service. This creates a virtual directory in IIS, and copies the .dll, .svc and .config files as required for the service to be Web hosted.

7. Open IIS Manager. Right-click the virtual directory (**ExtendedProtection**), which was created in the preceding step. Select **Convert to Application**.

8. Open the **Authentication** module in IIS Manager for this virtual directory and enable **Windows Authentication**.

9. Open **Advanced Settings** under **Windows Authentication** for this virtual directory and set it to **Required**.

10. You can test the service by accessing the HTTPS URL from a browser window (Provide a fully-qualified domain name). If you want to access this URL from a remote machine, make sure that the firewall is opened for all incoming HTTP and HTTPS connections.

11. Open the client configuration file and provide a fully-qualified domain name for the client or endpoint address attribute that replaces `<<full_machine_name>>`.

12. Run the client. The client communicates with the service, which establishes a secure channel and uses extended protection.