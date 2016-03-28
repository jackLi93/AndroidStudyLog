##Android之数据储存001
-  **001篇总结了SharedPreferences和File的使用。**

Android中的数据储存主要包括如下几项：

-  在SharedPreferences中保存简单数据类型的键值对
-  在Android的文件系统在保存任意文件，即通过file来保存数据
-  使用SQLite数据库来持久化数据


###SharedPreferences
- Android中SharedPreferences API用于轻量级简单数据类型的数据保存
- 一般保存APP配置，用户设置，是否登录
- 获取SharedPreferences对象，用于读取数据
<pre>
<code>	Context context = getActivity();
	SharedPreferences sharedPref = context.getSharedPreferences(
        getString(R.string.preference_file_key), Context.MODE_PRIVATE);
</code>
</pre>
- 向SharedPreferences中存入数据，使用SharedPreferences.Editor对象。

<pre>
<code>
SharedPreferences sharedPref = getActivity().getPreferences(Context.MODE_PRIVATE);
SharedPreferences.Editor editor = sharedPref.edit();
editor.putInt(getString(R.string.saved_high_score), newHighScore);
editor.commit();
</code>
</pre>
	-  注意：editor.commit()方法一定要调用，不然写入数据无效。
	
---
###使用File API来保存数据
**关于File存储**

-  File对象适合从开始到结束的顺序读取和写入大量数据。
-  比如，它适合图像文本文件，视频音频文件或者通过网络交换的任何内容。
-  与Java中的File IO系统有点相似。

**选择内部或者外部存储**

所有的Android设备都有两个文件存储区域：内部存储或者外部存储，外部存储常见的是外置SD卡。

**内部存储：**

-  始终可用
-  默认情况下只有您的应用可以访问此处保存的文件
-  当您的应用卸载的时候，数据也自动删除
-  当你希望确保用户或者其他应用均无法访问您的文件时，内部存储是最佳选择。

**外部存储**

-  并非始终可用，当SD卡坏掉或者被移除的时候无法使用
-  是全局可读的，比如下载的视频可用被很多播放器所打开
-  当用户卸载应用的时候，只有您通过getExternalFilesDir()将您的应用的文件保存在目录中时，系统才会删除文件
-  对于，无需访问限制而且希望与其他应用共享比如视频文本照片，外部存储是最佳的选择。
-  外部存储需要获取存储权限：
<code>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
</code>
如果您的应用需要读取外部存储，需要添加权限：
<code>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
</code>
---
**将文件保存在内部存储中**

- getFilesDir()：返回您的应用的内部目录的File
-  getCacheDir():返回您的应用的缓存文件的内部目录的File。
-  注意：务必删除所有不再需要的文件并合理制定缓存文件的大小，比如1MB。因为在系统内存不足的情况下，它将会在不进行警告的情况下删除您的缓存文件。
-  要在这些目录中新建一些文件，可以使用File()构造函数，传递指定您的应用内部文件目录和想创建的文件名。然后打开流写入数据。
<code>
File file = new File(context.getFilesDir(),filename);
</code>
- 看个写入数据的例子：
<pre><code>
String filename = "myfile";
String string = "Hello world!";
FileOutputStream outputStream;

try {
  outputStream = openFileOutput(filename, Context.MODE_PRIVATE);
  outputStream.write(string.getBytes());
  outputStream.close();
} catch (Exception e) {
  e.printStackTrace();
}
</code>
</pre>
-  获取应用的内部缓存，获取之后就能写入缓存数据
<pre>
<code>
public File getTempFile(Context context, String url) {
    File file;
    try {
        String fileName = Uri.parse(url).getLastPathSegment();
        file = File.createTempFile(fileName, null, context.getCacheDir());
    catch (IOException e) {
        // Error while creating file
    }
    return file;
}
</code>
</pre>

---
**将文件保存在外部存储中**

-  第一步：判断Android是否具有外部存储空间，即是否具有SD卡，以及SD卡是否具有空间。
<pre>
<code>
/* Checks if external storage is available for read and write */
public boolean isExternalStorageWritable() {
    String state = Environment.getExternalStorageState();
    if (Environment.MEDIA_MOUNTED.equals(state)) {
        return true;
    }
    return false;
}

/* Checks if external storage is available to at least read */
public boolean isExternalStorageReadable() {
    String state = Environment.getExternalStorageState();
    if (Environment.MEDIA_MOUNTED.equals(state) ||
        Environment.MEDIA_MOUNTED_READ_ONLY.equals(state)) {
        return true;
    }
    return false;
}
</code>
</pre>
-  如果能够读取和写入外部文件，那么需要考虑的是：可以保存两类文件：
	-  公共文件：可以供其他应用和用户使用的文件，应用卸载后仍然可以使用这些文件。
	-  私有文件：本属于您的应用且用户卸载时删除的文件。实际上不向您的应用之外的用户提供值的文件，但注意：因为存在外部存储上仍有技术可以被其他应用获取使用。
- 获取公共存储文件目录方法：getExternalStoragePublicDirectory()
- 获取私有外部存储文件目录：getExternalFilesDir()，通过此方法创建的各目录位于封装您应用的所有外部存储文件的父目录，应用卸载时，系统会删除这些文件。
- 看代码：第一个为创建公共相册第二个为创建私有个人相册：
	-  公有相册
<pre>
<code>
public File getAlbumStorageDir(String albumName) {
    // Get the directory for the user's public pictures directory. 
    File file = new File(Environment.getExternalStoragePublicDirectory(
            Environment.DIRECTORY_PICTURES), albumName);
    if (!file.mkdirs()) {
        Log.e(LOG_TAG, "Directory not created");
    }
    return file;
}
</code>
</pre>
	-  私有相册
<pre>
<code>
public File getAlbumStorageDir(Context context, String albumName) {
    // Get the directory for the app's private pictures directory. 
    File file = new File(context.getExternalFilesDir(
            Environment.DIRECTORY_PICTURES), albumName);
    if (!file.mkdirs()) {
        Log.e(LOG_TAG, "Directory not created");
    }
    return file;
}
</code>
</pre>
---

**查询可用空间**

注意：在保存文件之前，您无需查询可用空间量。当空间不足而你再写入文件时会出现IOException，将其捕获进行处理就ok了。

---
**删除文件：**

-  删除文件最直接方式，是获取file对象，调用其的delete方法，如：
	myFile.delete();
-  如果文件保存在内部存储中，还可以使用Context的deleteFile来定位和删除文件，比如：
	mContext.deleteFile(fileName);
-  当存在内部存储和私有的外部存储时，当应用被卸载的时候，数据会被自动删除。
-  另外应该手动删除使用getCacheDir()的所有的缓存文件和定期删除其他不需要的文件。
