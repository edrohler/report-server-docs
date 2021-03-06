---
title: IIS Configuration
page_title: IIS Configuration
description: IIS Configuration
slug: iis-configuration
tags: iis
published: true
position: 203
---

# IIS Configuration

The Report Server installer performs basic setup of the new Report Server web site under port 83. It is recommended to apply additional settings in IIS in order to compress resources sent from Report Server to the clients and improve load times.

The first step is to enable static and dynamic HTTP compression for the Report Server web site. A detailed guide is available in the [HTTP Compression](https://docs.microsoft.com/en-us/iis/configuration/system.webserver/httpcompression/) MSDN article.

The second step is to allow dynamic HTTP compression to compress application/json responses. This step is recommended when the Web Report Designer is enabled in Report Server and the designer requests large reports from the server. Large reports are considered to be reports containing many report items or embedded resources, such as images or CSV/JSON data.
Allowing this type of compression is done in [ApplicationHost.config](https://docs.microsoft.com/en-us/iis/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig). The location of the file is in the *%windir%\system32\inetsrv\config* directory. Add the following mime type to *httpCompression > dynamicTypes*:
`````
	<httpCompression directory="%TEMP%\iisexpress\IIS Temporary Compressed Files">
		<scheme name="gzip" dll="%IIS_BIN%\gzip.dll" />
		<dynamicTypes>
			...

			<!-- compress JSON responses from Web API -->           
			<add mimeType="application/json" enabled="true" /> 

			...
		</dynamicTypes>
		<staticTypes>
			...
		</staticTypes>
	</httpCompression>
`````

It is recommended to stop IIS before applying changes to the [ApplicationHost.config](https://docs.microsoft.com/en-us/iis/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig).
