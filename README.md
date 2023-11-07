<h1><img src="https://user-images.githubusercontent.com/6410412/280930996-ab5931f7-1f1f-42b9-a243-63d1d9c3ada4.png" width="22px" height="22px" /> Zap</h1>

Zap is a development library for building multi-device application that enable communication with other devices. While mobile devices offer a wide range of data sources, such as motion sensors, biometrics devices, microphones, touchscreens and more, traditional PCs like laptops and desktops are typically lack these resources.

The data sources available on mobile devices are valuable, but are often device-dependent, limiting their widespread use. Imagine if PCs could use the series of data from the accelerometer sensor on a mobile device.

Zap provides interfaces to access data sources on other devices. In the following example shows that the client instance on an Android device sends acceleration force data to the server device.

```kotlin
class MainActivity: AppCompatActivity(), SensorEventListener {
  private lateinit var zap: ZapClient

  override fun onCreate(state: Bundle?) {
    // Create a new zap client with the server's IP address.
    zap = ZapClient(InetAddress.getByName(...))
  }

  // Define the method that is invoked whenever
  //   any sensors on the local device have changed.
  override fun onSensorChanged(event: SensorEvent) {
    if (event.sensor.type == Sensor.TYPE_ACCELEROMETER) {
      val (x, y) = event.values
      // Send the data acquired from the accelerometer to the server.
      zap.send(ZapAccelerometerData(x, y))
    }
  }
}
```

> [!NOTE]
> Currently, Zap does not provide a built-in methods for obtaining the server's IP address. To identify the server device's address, you may consider alternative way such as using QR code to share the server's address or communicating via protocols like NFC or BLE.

On the server device, the server instance can retrieve the data from accelerometer sensor on the client device. In this example, TypeScript code is executed on the Node.js runtime on a desktop.

```javascript
// Create and start a new zap server to listen for data from clients.
new class extends ZapServer {
  // Define the method that is called whenever accelerometer sensor data is
  //   received from client devices.
  onAccelerometerChanged(_: string, x: number, y: number) {
    console.log(`Data received, x: ${x}, y: ${y}`);
  }
}.start();
```

The main goal of Zap is to support mobile-PC communication, but it also extends its capabilities to enable mobile-mobile and PC-PC communication. Furthermore, it's not limited to PCs; any devices capable of running Zap implementations(e.g., Kiosk device, Smart TV, etc.) can also participate in this communication.

## Example Applications

- [Simple client-server (Android)](https://github.com/zap-lib/examples/tree/main/android-client-server): In this example, an Android mobile device performs as both the client and server. The acceleration data received from the client device is used to move a small circle on the server device.
- [Cursor controller (Node.js)](https://github.com/zap-lib/examples/tree/main/node-cursor-controller): A example demonstrating how Node.js Zap server can receive acceleration data from an Android mobile device and use it to move a cursor.

See more examples on [zap-lib/examples](https://github.com/zap-lib/examples) repository.

## Implementations

- [Kotlin](https://github.com/zap-lib/kotlin)
- [Node.js](https://github.com/zap-lib/node)
