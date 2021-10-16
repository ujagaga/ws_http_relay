# ws_http_relay

A flask based website (web service) for message exchange using WEB SOCKETS and HTTP GET requests.
I often build small embedded evices based on ESP8266. I do not like relying on a third party product, but prefer using my own. 
I needed a way to communicate with a device running bahind a NAT, so no public IP.
Also, I want it to be free. When this is finished anyone will be able to use it as they see fit with a couple of restrictions:

1. There will be a trafic limit from one IP to avoid website crashing due to brut force attack.
2. this solution does not implement much security, so it is up to you to add security measures to communication protocol.
3. It is advised to use HTTPS if possible to disable the possibility of trafic sniffing.

## How to use

A device you wish to controll should connect via web socket to the root address of this service.
It should then send the registration request like:


	"id=<some_dev_id>"
	
	
When you wish to send data to this device, you simply need to make a get request to: 


	http://<web_service_url>?id=<device_ID>&some_var=your_data&another_var=other_data
	

If a device is connected to web socket and is listening to message topic <device_ID>, it will receive via websocket, the json formated request:


	'{"some_var":"your_data", "another_var":"other_data"}'
	
	
If the device responds within 10s timeout with the same id like:


	{"id": "<device_id>", "some_var": "some value"}
	
	
this info will be propagated back to the client which made the get request.
This way, you can make a two way communication, by making an HTTP GET request to specific URL and receiving the device response back.
As you can see, the only security here is the device ID, so choose it carefully. 
As a further security measure, you can add a specific token that your device expects. 
Without this token your device should not respond, which will make the id seem like there is no device available.

## Device Response Format

As you have previously seen, the device should respond in json format so it can send its ID and additional data. Some Json keys that the service will look for are:

	1. "type": "<response type>". This can be any web supported type like: html, text, raw, image,...
	2. "content": "<html, text or byte content to return via http>".
	3. "status": <numeric status code>. This code can be any number you wish. If it is not a number, the service will ignore it and send "OK/200"
	
All other keys will be considered a part of "content" value and will be send as Json array.
	

## status

Just started development, so not ready yet for testing. The description above is a preliminary idea and will be updated as it gets developed.

## Contact ##

* web: http://www.radinaradionica.com
* email: ujagaga@gmail.com
