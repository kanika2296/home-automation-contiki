# Home Automation simualtion using Contiki
Simulation of smart home automation system devices using contiki OS __InstantContiki 2.7__

The Home automation IoT network is created using Cooja, Contiki’s IoT simulator. It uses IoT protocols
6lowpan from communicating over ipv6 and CoAP application layer protocols using which sensors share value
to a CoAP client. We have used Sky mote to create sensors. Tmote Sky is an ultra-low power wireless module
for use in sensor networks, control applications and rapid prototyping applications. Sky motes are used to
emulate Tmotes in Cooja. Home automation network is connected to the outside internet using Contiki’s RPL
Border Router.

By default, border router hosts a simple web page, which displays IPv6 addresses of all the nodes connected to
the router. To add a border router to the network:
1. Run cooja using Ant Run
2. Compile border-router.c from ipv6/rpl-border-router/border-router.c
3. Then click on create to add a border router to the network.

We need to build a connection between the Cooja simulated RPL network and the local computer. This
can be achieved by right-clicking the mote that is programmed as a border router. Pick 'More tools for
border router' and then choose 'Serial Socket (SERVER).' We will receive the following message when
this phase is successfully completed.

To simulate connections of RPL border router to external network, we will use the Tunslip utility
provided in Contiki. To run Tunslip:
```
cd contiki/examples/ipv6/rpl-border-router
make connect-router-cooja
```
The home automation network consists of 6 different sensors build based on Cooja’s COAP server example
__er-rest-exampler-server.c__

The Erbium Server has light, led, switched, helloworld, and resource discovery resource has a resource handler
function that is called when the client requests the resource and the resource handler function sends the response
back to the client. We have added an additional handler, periodic to collect sensor data after short intervals. Such
sensors are called observable in Contiki.
The various sensors are:
1) __Solar Sensor__: Solar sensor is a type of light sensor which is extended from Cooja’s OOB light sensor
functionality. This sensor is periodic sensor which sends data to CoAP client every
60*CLOCK_SEOCNDS of time. The sensors in built using OOB coap server .c file available called
er-rest-example.c. To add light_solar.c sensor to home automation network, compile Sky mote , with light_solar.c (input
file for this sensor) and then click add. The sensor will be assigned an IPv6 address when it connects to
the border-router. Using this IP address, the sensor can be accessed on the CoAP client. In the client
window, a user is able to see the values a sensor sends, periodically changing.
2) __Temperature sensor__ : It is also periodical, based on the OOB shtt sensor of Cooja.
3) __Fan sensor__: This sensor is enabled with a button to provide turning on/off capabilities.It is also an observable
sensor which observes the click of a button. Current state of this sensor is represented as (1), which means fan is
on.
4) __Door and garage door sensor__ have configuration as fan sensors. In these sensors, initially doors are at state (0)
closed. On click of a button, the state changes to (1), implying the door was opened.
5) __Camera sensor__: This sensor is an observable resource, with a button. The camera is initially on state. Users can use
the button to modify the state i.e turn it off. Aslo attacker can modify the values camera records by sending in
malicious values using the post option in COAP client.
