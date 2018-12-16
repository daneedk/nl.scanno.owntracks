# Owntracks MQTT Client for Homey

The Owntracks MQTT Client for Homey is a location app that receives messages from Owntracks apps running on
your phone. This ways Homey knows when you enter or leave an area (GeoFence) you specified on your phone.
A GeoFence can be your home or workplace (or any place that you spicify on your phone). With the use of trigger
cards, you can trigger flows when you enter or leave a specific GeoFence.

## What is Owntracks
Owntracks is an open source location app for Android and iOS. It sends location information to a MQTT broker or
through the Athom Cloud by using HTTPS.

The Owntracks app does not use a lot of battery.

## HTTPS or MQTT?
Using HTTPS is for most users the easiest way to get started. You just need your homeys cloud-id, create a user in settings and
you good to go.

The most secure solution is to use your own MQTT broker or create a private one in the cloud. CloudMQTT is a MQTT
hosting service that has a free hosting plan. See my [tutorial](https://forum.athom.com/discussion/2810/use-cloudmqtt-as-a-broker-for-owntracks-or-mqtt-app)

## How does this work and what can it do on Homey?
In the Owntracks app you can add geofences, and Owntracks sends events to the broker or athom cloud containing information about
entering or leaving a geofence. These events can be used with trigger cards in Homey flows.
And as such it can be used for presence detection.

The Owntracks app sends its data to a MQTT topic on the broker. This topic is different for each user and each device.
A typical topic name would be: `owntracks/<user-id>/<device-name>/event`
The trailing event means that only events regarding geofences will be received by the Homey client. In the Owntracks
clients, you can find out what topic is used to send the messages to (do not forget to add event to that topic).
Besides subscribing to the MQTT topic, you have to enter the name of the geofence you specified in the Owntracks Android
or iOS app (regions section). Make sure you enbale the share option. Otherwise the name of the geofence is not included in
the message and the trigger will not fire.

This app supports the following trigger cards:
- a card that will trigger when entering the specified geofence
- a card that will trigger when leaving the specified geofence
- a card that will trigger on a enter / leave event on the specified geofence. This card provides a tag that contains
  the event (i.e. the values can be enter or leave)

This app supports the following activity cards:
- a card that let Homey speak out the current location of the user specified.

### Homey now supports speech. Ask Homey where a user is, and it will tell you the current name of the GeoFence the user is at.

The settings page contains:
- The option to use MQTT next to HTTPS (HTTPS is enabled as default)
- IP adres or DNS name of the broker wehre to connect top
- Portnumber to connect to.
- The option to use a secure session (TLS). No support for self signed certificates.
- Userid for the broker connection
- Password to use for the broker connection
- The ability to specify the loaction accuracy in meters. Default is set at 100 meters. If the accuracy is worse than
  100 meters, the received event will be ignored.

*Ingmar made a very nice [tutorial](https://forum.athom.com/discussion/2804/setup-homey-with-mqtt-for-location-detection/p1) on the workings of this app.*
