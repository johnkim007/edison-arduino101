# edison-arduino101
Intel edison-Arduino/Genuino 101


edison-arduino101-iot

Edison + Arduino 101 IoT demo

This demo illustrates a simple IoT example by displaying an Arduino 101's IMU (accelerometer / gyroscope) data on a web page. In order to do this, an Intel Edison module is used to receive the data from the Arduino 101 via BLE (Bluetooth Low Energy), then sends it to a web server via WebSockets.

Here's a picture that describes the overall architecture:

Archiecture

Arduino 101 uses the CurieBLE library to broadcast the IMU data via BLE. Edison is responsible for receiving this data using noble, a Node.js BLE central module, and sending it to a separate web server using socket.io-client. The web server—which also uses Node.js and socket.io—is responsible for receiving data from Edison and hosting a web page that displays the IMU data. For more details, see this blog post.

Setting up the demo

Edison

Installing software using opkg

Add AlexT's unofficial opkg repository. 
It contains many precompiled packages that can be installed by simply typing opkg install <package name>.

To configure the repository, add the following lines to /etc/opkg/base-feeds.conf:

src/gz all http://repo.opkg.net/edison/repo/all
src/gz edison http://repo.opkg.net/edison/repo/edison
src/gz core2-32 http://repo.opkg.net/edison/repo/core2-32

Update the package manager and install the required packages:
opkg update
opkg install git systemd-dev


Setting up Node.js

git clone https://github.com/drejkim/edison-arduino101-iot

Install async globally: npm -g install async
Navigate to edison/ and type npm install
Modify the socket variable in edison/sensors.js with your web server's URL—see note below
Web server

Note: Your web server should NOT be running on the Edison that's running sensors.js. 
It can run on a personal computer, or even on a different Edison.

Navigate to web/server/ and type npm install
Modify the socket variable in web/client/app.js with your web server's URL
Running the demo

Arduino 101

Upload the sketch located in arduino/imu/imu.ino to your Arduino 101. After a few seconds, it should be ready to go.

Edison

npm install bluetooth-hci-socket
Navigate to edison/ and type node sensors.js to start the program. The Edison console should look similar to this:

root@myedison:~# node sensors.js
Start BLE scan...
Connected to peripheral: 984fee0f3980
Discovered IMU service
ax:edison
ay:edison
az:edison
gx:edison
gy:edison
gz:edison
If the output only goes up to "Connected to peripheral...", try restarting the program after a few seconds. This can help in discovering the BLE service.

Web server & client

Start the web server by navigating to web/server/ and typing node server.js
Open a browser window and navigate to the web server's URL
You should now see the accelerometer / gyroscope data from your Arduino 101 streaming to the web page!

Troubleshooting

If you're not seeing the data change on the webpage, first check that the web server console looks something like this:

HTTP server listening on port 8080
Client connected
Also, check that the Edison console (where sensors.js is running) outputs a new line, Connected to server, and looks similar to this:

root@myedison:~# node sensors.js
Start BLE scan...
Connected to peripheral: 984fee0f3980
Discovered IMU service
ax:edison
ay:edison
az:edison
gx:edison
gy:edison
gz:edison
Connected to server
On the Arduino 101, go to Tools -> Serial Monitor. The data should be outputting to the serial monitor and look something like this:

Arduino 101 Serial Monitor
Contact GitHub 
