# iot-central-web-mqtt-device
Create a simple IoT device in a web browser that communicates with Azure IoT Central


## Introduction

This project is based in large part on the work done by Github user [ridomin](https://github.com/ridomin) and the repository [iothub-webclient](https://github.com/ridomin/iothub-webclient).  In this project I have focused the sample on connecting to an IoT Central application and it exclusively uses the Device Provisioning Service (DPS) for the provisioning and connection of the device.  IoT Central does not support connection strings and requires the device to use DPS to obtain it the location of it's IoT hub, this allows a device to be moved between hubs and in the near futrure for IoT Central to support IoT Hub redundancy.  The device running in the browser is also setup to run as an autonomous device very much like if the device was running in the real world.  The devices two way communication between IoT Central and itsel;f can be observed in the console output on the web page once sending telemetry has been started.

The device supports the following functionality:

* Connection to IoT Central using DPS.  The device does not need to be registered in the application prior to connecting
* The device can be authenticated via either the device SAS token or the Group SAS token for the application (in this case the device SAS token is automatically generated)
* The device is automatically associated with the model identity provided by the user in the form
* Sending the telemetry values for temperature and humidity (randomly generated values in a range) ever ~5 seconds
* Sending the reported property of fan speed to IoT Central every ~10 seconds
* Accepting a desired (writable) property setTemp to set the temperature of a thermostat on the device
* Accepting the direct method call sendTextMessage from IoT Central with a text message and responding with a boolean return value if the message is read
* Accepting a cloud to device message startVacuumCleaner that takes a time value as a parameter (this is additional functionality over iothub-webclient version)

All communication and payloads are displayed on the web browser console output with the last 200 transmissions held in history.


## Hosting locally

To host this locally clone this repository to your computer and run the following:

``` 
npm install
npm start
```

The following should be seen after running the last command

```
> webmqtt@1.0.0 start D:\github\iot-central-web-mqtt-device
> node server.js

Listening at http//localhost:8080
```

If you see an error check to see if the port 8080 is already in use on your machine and if it is feel free to change the port to a free one by changing the code in server.js on line 23.

Open your browser of choice and go to the URL http//localhost:8080.  You should see the following in you browser:

![Initial browser screen](https://github.com/iot-for-all/iot-central-web-mqtt-device/blob/main/assets/initialscreen.png "Initial browser screen")


## Hosting on Azure

The code can obviously be hosted on any web service but in my case I chose to host using an [Azure function](https://azure.microsoft.com/en-us/services/functions/?&ef_id=CjwKCAiAsOmABhAwEiwAEBR0ZmNO6WIwjimRlpY2W-N4U_G99qJHALIQa-hykDyFhzNSz6bJl3x8nRoCVcYQAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiAsOmABhAwEiwAEBR0ZmNO6WIwjimRlpY2W-N4U_G99qJHALIQa-hykDyFhzNSz6bJl3x8nRoCVcYQAvD_BwE:G:s&gclid=CjwKCAiAsOmABhAwEiwAEBR0ZmNO6WIwjimRlpY2W-N4U_G99qJHALIQa-hykDyFhzNSz6bJl3x8nRoCVcYQAvD_BwE) as it is a cost effective way to host infrequently accessed static web pages.

I used [this blog post](https://www.wintellect.com/host-website-azure-functions-node-js-part-1/) to setup my Azure function.  The code needed for the Azure Function can be found in the file [azure-function/index.js](https://github.com/iot-for-all/iot-central-web-mqtt-device/blob/main/azure-function/index.js).  

You will need to copy the contents of the content directory up to your Azure Function location and also install the needed npm package mime-types.  All this is explained in the blog post.  Once you have followed the insdtructions you should be able to browse to your azure functions URL with https://<your-function-name>.azurewebsites.net/device.html and you should see the following in your browser:

![Initial browser screen](https://github.com/iot-for-all/iot-central-web-mqtt-device/blob/main/assets/initialscreen.png "Initial browser screen")


## Running the sample