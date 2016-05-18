### EGL _ BAD _ SURFACE导致Crash的解决办法

---

-  在三星平板上打开应用程序，不做任何操作，过一段时间自动Crash掉。

-  设备信息：Android 4.4.2 三星平板名称：SM-P600


**Crash log:**

---
05-16 16:18:41.947: I/connectwifi(22249): wifi connect start

05-16 16:18:42.817: I/connectwifi(22249): wifi connect start

05-16 16:18:51.947: I/connectwifi(22249): wifi connect start

05-16 16:18:52.817: I/connectwifi(22249): wifi connect start

05-16 16:19:01.947: I/connectwifi(22249): wifi connect start

05-16 16:19:02.822: I/connectwifi(22249): wifi connect start

05-16 16:19:11.947: I/connectwifi(22249): wifi connect start

05-16 16:19:12.822: I/connectwifi(22249): wifi connect start

05-16 16:19:21.947: I/connectwifi(22249): wifi connect start

05-16 16:19:22.822: I/connectwifi(22249): wifi connect start

05-16 16:19:31.952: I/connectwifi(22249): wifi connect start

05-16 16:19:32.822: I/connectwifi(22249): wifi connect start

05-16 16:19:41.947: I/connectwifi(22249): wifi connect start

05-16 16:19:42.817: I/connectwifi(22249): wifi connect start

05-16 16:19:51.947: I/connectwifi(22249): wifi connect start

05-16 16:19:52.817: I/connectwifi(22249): wifi connect start

05-16 16:20:01.947: I/connectwifi(22249): wifi connect start

05-16 16:20:02.822: I/connectwifi(22249): wifi connect start

05-16 16:20:02.837: W/HardwareRenderer(22249): EGL error: EGL _ BAD _ SURFACE

05-16 16:20:02.852: W/HardwareRenderer(22249): Mountain View, we've had a problem here. Switching back to software rendering.

05-16 16:20:11.952: I/connectwifi(22249): wifi connect start

05-16 16:20:11.952: W/dalvikvm(22249): threadid=18: thread exiting with uncaught exception (group=0x417a3c08)

05-16 16:20:11.957: E/JavaBinder(22249): !!! FAILED BINDER TRANSACTION !!!

05-16 16:20:11.957: D/AndroidRuntime(22249): Shutting down VM

05-16 16:20:11.957: W/dalvikvm(22249): threadid=1: thread exiting with uncaught exception (group=0x417a3c08)

05-16 16:20:11.957: I/Process(22249): Sending signal. PID: 22249 SIG: 9

**解决办法**

-  在Manifest文件中将对应的Activity中的hardwareAccelerated设置为false;
-  android:hardwareAccelerated="false"

**参考文档：**

-  [EGL error: EGL _ BAD _ SURFACE](https://code.google.com/p/android/issues/detail?id=63738)
-  [ Android 4.0的图形硬件加速及绘制技巧](http://blog.csdn.net/internetman/article/details/7098363)

For me 
android:hardwareAccelerated="false"
in the Manifest fixes the crash. Nevertheless this causes other strange behaviours in my App.