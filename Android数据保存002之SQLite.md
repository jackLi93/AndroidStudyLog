##Android数据保存002之SQLite

**前言**

-  将数据保存到数据库对于大量，重复和结构化数据而言是理想之选。
-  本节假定你熟悉基本SQL数据库知识
-  Android中使用数据库所需要的API在android.database.sqlite包中。

---
**What is SQLite ?**

SQLite 是一个轻量级开源的SQL关系型数据库，简单易用。它的特性很适合在移动设备上使用，故IOS和Android平台都内置了SQLite数据库。 

**Why is SQLite ?**

-  简单易用
-  关系型数据库支持SQL语句
-  开源免费

**How to use SQLite ?**

**使用步骤，三步走**
 
1.  定义数据库的架构和契约
2.  使用SQLiteOpenHelper创建数据库
3.  对数据库进行增删改查的操作

---

####定义数据库的架构和契约
SQL数据库的主要原则之一是架构，即数据库如何组织声明。而数据库的架构和您业务息息相关，一旦创建好了契约类之后将有助于SQL语句的创建和数据库的构建与使用。

所谓的契约类：简单理解就是将和业务相关的名称字段独立封装起来供数据库帮助类以及相关操作直接使用。

契约类是全局级别的类有点类似于工具类。比如以下代码：
<pre>
<code>
public final class FeedReaderContract {
    // To prevent someone from accidentally instantiating the contract class,
    // give it an empty constructor.
    public FeedReaderContract() {}

    /* Inner class that defines the table contents */
    public static abstract class FeedEntry implements BaseColumns {
        public static final String TABLE_NAME = "entry";
        public static final String COLUMN_NAME_ENTRY_ID = "entryid";
        public static final String COLUMN_NAME_TITLE = "title";
        public static final String COLUMN_NAME_SUBTITLE = "subtitle";
        ...
    }
}
</code>
</pre>

**代码解读：**

-  该代码定义了单个表格的表名称和列名称，都是final String类型的，即定义了数据库的表结构。
-  我们知道，关系型数据库就是若干张表的相互关系。
-  通过实现BaseColumns接口，实现主键字段_ID，非必须
-  将业务逻辑与数据库创建分离，具有很强的扩展性

---

###使用SQLiteOpenHelper创建和维护数据库
定义了数据库的表结构之后，接下来就是创建和维护数据库和表格的方法了。这里会用到一些经典的SQL语句：
比如创建表，比如删除表：
<pre>
<code>
private static final String TEXT_TYPE = " TEXT";
private static final String COMMA_SEP = ",";
private static final String SQL_CREATE_ENTRIES =
    "CREATE TABLE " + FeedEntry.TABLE_NAME + " (" +
    FeedEntry._ID + " INTEGER PRIMARY KEY," +
    FeedEntry.COLUMN_NAME_ENTRY_ID + TEXT_TYPE + COMMA_SEP +
    FeedEntry.COLUMN_NAME_TITLE + TEXT_TYPE + COMMA_SEP +
    ... // Any other options for the CREATE command
    " )";

private static final String SQL_DELETE_ENTRIES =
    "DROP TABLE IF EXISTS " + FeedEntry.TABLE_NAME;
</code>
</pre>


-  SQLiteOpenHelper类中有一组有用的API，要使用SQLiteOpenHelper请创建一个重写了onCreate()，onUpgrade(),onOpen()回调的子类。可能还需要onDowngradle()，但这并非必须。
-  注意：由于他们可能长期运行，确保您在后台线程中调用getWritableDatabase() 或 getReadableDatabase() ， 比如使用 AsyncTask 或 IntentService。
-  一个简单的例子：
<pre>
<code>
import android.content.Context;  
import android.database.sqlite.SQLiteDatabase;  
import android.database.sqlite.SQLiteDatabase.CursorFactory;  
import android.database.sqlite.SQLiteOpenHelper;  
  
public class DbHelper extends SQLiteOpenHelper{  
  
    public DbHelper(Context context, String name, CursorFactory factory,  
            int version) {  
        super(context, name, factory, version);  
        // TODO Auto-generated constructor stub  
    }  
  
    @Override  
    public void onCreate(SQLiteDatabase db) {  
        // TODO Auto-generated method stub  
          
    }  
  
    @Override  
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {  
        // TODO Auto-generated method stub  
          
    }  
  
}  
</code>
</pre>

-  Google开发者培训里的例子：
<pre>
<code>
public class FeedReaderDbHelper extends SQLiteOpenHelper {
    // If you change the database schema, you must increment the database version.
    public static final int DATABASE_VERSION = 1;
    public static final String DATABASE_NAME = "FeedReader.db";

    public FeedReaderDbHelper(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }
    public void onCreate(SQLiteDatabase db) {
        db.execSQL(SQL_CREATE_ENTRIES);
    }
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        // This database is only a cache for online data, so its upgrade policy is
        // to simply to discard the data and start over
        db.execSQL(SQL_DELETE_ENTRIES);
        onCreate(db);
    }
    public void onDowngrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        onUpgrade(db, oldVersion, newVersion);
    }
}
</code>
</pre>
**代码学习：**

-  可以看出Google给的代码很规范而又简洁，将所有不能改变的东西都设置为static final的类型。
-  构造函数调用了父类的构造函数，创建数据库
-  重写了onCreate方法，执行创建表格的语句，数据库第一次创建的时候onCreate会被调用
-  重写了升级数据库版本的方法

###获取数据库，并对数据库进行增删改查的数据操作
<pre>
<code>
FeedReaderDbHelper mDbHelper = new FeedReaderDbHelper(getContext()); //01
// Gets the data repository in write mode
SQLiteDatabase db = mDbHelper.getWritableDatabase();//02
</code>
</pre>
-  01创建继承了SQLiteOpenHelper数据库帮助类
-  02获取SQLiteDatabse的对象，数据库对象的获取只能通过数据库帮助类的getReadableDatabase()(只读)或者getWritableDatabase()(只写)来获取。

####向数据库存入数据
将数据插入到数据库，需要用到ContentValues对象并调用insert()方法：
<pre>
<code>
//get db in write mode
SQLiteDatabse db = mDbHelper.getWritableDatabase();

//create a new map of values.put <key,values>。
ContentValues values = new ContentValues();
values.put(FeedEntry.COLUMN_NAME_ENTRY_ID, id);
values.put(FeedEntry.COLUMN_NAME_TITLE, title);
values.put(FeedEntry.COLUMN_NAME_CONTENT, content);

//insert the values，returning the primary key value of the new row
long newRowId;
newRowId = db.insert(
         FeedEntry.TABLE_NAME,
         FeedEntry.COLUMN_NAME_NULLABLE,
         values);
</code>
</pre>
**代码解读**

- insert的第一个参数为表格的名称，第二个参数指定在ContentValues为空的情况下框架可在其中插入NULL的列的名称，如果将其设置为null,不能插入。

-  
####读取数据库数据
要从数据库中读取数据，使用query()方法，调用该方法将会返回一个Cursor的对象给您。Cursor是一系列对象的合集，具有相关的方法供我们调用，一般将Cursor里的数据遍历出来然后显示。
<pre>
<code>
查询数据库的方法
SQLiteDatabase db = mDbHelper.getReadableDatabase();

// Define a projection that specifies which columns from the database
// you will actually use after this query.
String[] projection = {
    FeedEntry._ID,
    FeedEntry.COLUMN_NAME_TITLE,
    FeedEntry.COLUMN_NAME_UPDATED,
    ...
    };

// How you want the results sorted in the resulting Cursor
String sortOrder =
    FeedEntry.COLUMN_NAME_UPDATED + " DESC";

Cursor c = db.query(
    FeedEntry.TABLE_NAME,  // The table to query
    projection,                               // The columns to return
    selection,                                // The columns for the WHERE clause
    selectionArgs,                            // The values for the WHERE clause
    null,                                     // don't group the rows
    null,                                     // don't filter by row groups
    sortOrder                                 // The sort order
    );
</code>
</pre>
<pre>
<code>
遍历Cursor的方法
// create Cursor in order to parse our sqlite results
			Cursor cursor = sqliteDB.rawQuery("SELECT bookTitle FROM " + tableName, null);
			// if Cursor is contains results
			if (cursor != null) {
				// move cursor to first row
				if (cursor.moveToFirst()) {
					do {
						// Get version from Cursor
						String bookName = cursor.getString(cursor.getColumnIndex("bookTitle"));

						// add the bookName into the bookTitles ArrayList
						bookTitles.add(bookName);
						// move to next row
					} while (cursor.moveToNext());
				}
			}
</code>
</pre>

####删除数据库数据
开发者文档给出的实例，但是我觉得也许直接使用db.execSQL()更直观简单一些。
<pre>
<code>
// Define 'where' part of query.
String selection = FeedEntry.COLUMN_NAME_ENTRY_ID + " LIKE ?";
// Specify arguments in placeholder order.
String[] selectionArgs = { String.valueOf(rowId) };
// Issue SQL statement.
db.delete(table_name, selection, selectionArgs);
</code>
</pre>
####更新数据库数据
<pre>
<code>
SQLiteDatabase db = mDbHelper.getReadableDatabase();

// New value for one column
ContentValues values = new ContentValues();
values.put(FeedEntry.COLUMN_NAME_TITLE, title);

// Which row to update, based on the ID
String selection = FeedEntry.COLUMN_NAME_ENTRY_ID + " LIKE ?";
String[] selectionArgs = { String.valueOf(rowId) };

int count = db.update(
    FeedReaderDbHelper.FeedEntry.TABLE_NAME,
    values,
    selection,
    selectionArgs);
</code>
</pre>
####实践中使用数据库
-  在实践中使用数据库时，往往会结合业务逻辑对数据的增删改查进行监听，当监听数据的改变，实现相应的业务逻辑。比如：在设备管理类DeviceManager中会有一个接口:public interface dataChangedListener{....}并封装了dbHelper;
<pre>
<code>
public class DeviceManager{
....
public interface dataChangedListener{
....
void dataAdded();
void dataDeleted(String name);
void dataUpdated(String oldName,String newName);
...
}
....

}
</code>
</pre>
-  使用SQLite数据库的增删改查操作都能用两种方式来实现：1是调用封装好的相关方法，2是执行SQL语句。
-  SQLite数据库操作完成后一定要及时关闭。代码如下：
<pre>
<code>
	/**
	 * close the database 关闭数据库
	 */
	public void close() {
		if (this.db != null) {
			db.close();
			db = null;
		}
	}
</code>
</pre>