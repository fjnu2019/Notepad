# hznotepad
notepad期中实验

一、内容简介

1、基本要求：

1）显示条目增加时间戳显示

2）添加笔记查询功能

2、附加功能

1）背景美化

更改了背景图片，美化了记事本

2）字体大小更换

可以改变字体的大小，有小、中、大三种大小

3）字体颜色更换

可以更换字体的颜色

4）导出功能

可以把内容导出至手机SD卡中

5）排序功能

可以按照书写顺序或者修改顺序将笔记排序

二、关键步骤代码及截图

1、NoteList中显示条目增加时间戳显示
1)添加显示时间的TextView在layout的布局文件notelist_item.xml中。
```
<TextView
    android:id="@+id/text1"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:gravity="center_vertical"
    android:paddingLeft="10dip"
    android:singleLine="true"
    android:layout_weight="1"
    android:layout_margin="0dp"
    />

<TextView
    android:id="@+id/text2"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:textSize="12dp"
    android:gravity="center_vertical"
    android:paddingLeft="10dip"
    android:singleLine="true"
    android:layout_weight="1"
    android:layout_margin="0dp"
    />
   ```
2)NoteEditor.java中创建和修改时间的两个字段代码如下
```
private final void updateNote(String text, String title) {
    Date nowTime = new Date(System.currentTimeMillis());
   // System.out.println(System.currentTimeMillis());
    SimpleDateFormat sdFormatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    String retStrFormatNowDate = sdFormatter.format(nowTime);
    //System.out.println(retStrFormatNowDate);
```
3)NodeEditor.java中将时间存入数据库的代码如下所示
```
ContentValues values = new ContentValues();
values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, retStrFormatNowDate);
```
4)NoteList.java中的字段描述如下
```
adapter
    = new SimpleCursorAdapter(
              this,                             // The Context for the ListView
              R.layout.noteslist_item,          // Points to the XML for a list item
              cursor,                           // The cursor to get items from
              dataColumns,
              viewIDs,
              CursorAdapter.FLAG_REGISTER_CONTENT_OBSERVER
      );

private String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE ,NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE} ;

private int[] viewIDs = { R.id.text1,R.id.text2 };
```
截图如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190519140223186.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NhbW11cmFtYXQ=,size_16,color_FFFFFF,t_70)

2.添加笔记查询功能
1)NodeList.java中查询功能的实现关键代码
```
private void SearchView(){
    searchView=findViewById(R.id.sv);
    searchView.onActionViewExpanded();
    searchView.setQueryHint("搜索笔记");
    searchView.setSubmitButtonEnabled(true);
    searchView.setOnQueryTextListener(new SearchView.OnQueryTextListener() {
        @Override
        public boolean onQueryTextSubmit(String s) {
            return false;
        }

        @Override
        public boolean onQueryTextChange(String s) {
            if(!s.equals("")){
                String selection=NotePad.Notes.COLUMN_NAME_TITLE+" GLOB '*"+s+"*'";
                updatecursor = getContentResolver().query(
                        getIntent().getData(),            // Use the default content URI for the provider.
                        PROJECTION,                       // Return the note ID and title for each note.
                        selection,                             // No where clause, return all records.
                        null,                             // No where clause, therefore no where column values.
                        NotePad.Notes.DEFAULT_SORT_ORDER  // Use the default sort order.
                );
                if(updatecursor.moveToNext())
                    Log.i("daawdwad",selection);
            }
           else {
                updatecursor = getContentResolver().query(
                        getIntent().getData(),            // Use the default content URI for the provider.
                        PROJECTION,                       // Return the note ID and title for each note.
                        null,                             // No where clause, return all records.
                        null,                             // No where clause, therefore no where column values.
                        NotePad.Notes.DEFAULT_SORT_ORDER  // Use the default sort order.
                );
            }
            adapter.swapCursor(updatecursor);

           // adapter.notifyDataSetChanged();
            return false;
        }
    });
}
```
2)listview.xml中的控件
```
<android.support.v7.widget.SearchView
    android:id="@+id/sv"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    >
  ```

结果截图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190519140635975.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NhbW11cmFtYXQ=,size_16,color_FFFFFF,t_70)

查找2

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190519140715327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NhbW11cmFtYXQ=,size_16,color_FFFFFF,t_70)

查找han

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190519140801199.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NhbW11cmFtYXQ=,size_16,color_FFFFFF,t_70)

3.背景美化：
关键代码:
```
android:background="@drawable/i"
android:background="@drawable/m"
```
截图如下:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190519142701259.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NhbW11cmFtYXQ=,size_16,color_FFFFFF,t_70)

4.字体大小更换
1)NoteEditor中添加如下代码:
```
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    // Handle all of the possible menu actions.
    switch (item.getItemId()) {
        case R.id.menu_save:
            String text = mText.getText().toString();
            updateNote(text, null);
            finish();
            break;
        case R.id.menu_delete:
            deleteNote();
            finish();
            break;
        case R.id.menu_revert:
            cancelNote();
            break;
        case R.id.red_font:
            mText.setTextColor(Color.RED);
            break;
        case R.id.black_font:
            mText.setTextColor(Color.BLACK);
        case R.id.font_10:
            mText.setTextSize(20);
            break;
        case R.id.font_16:
            mText.setTextSize(32);
            break;
        case R.id.font_20:
            mText.setTextSize(40);
            break;
    }
    return super.onOptionsItemSelected(item);
}
```
2)editor_options_menu.xml中添加如下代码:
```
<item android:title="字体大小">
    <menu>
        <group>
            <item android:id="@+id/font_10"
                android:title="小"/>
            <item android:id="@+id/font_16"
                android:title="中"/>
            <item android:id="@+id/font_20"
                android:title="大"/>
        </group>
    </menu>
</item>
```

截图如下所示:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190519143037459.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NhbW11cmFtYXQ=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190519143046983.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NhbW11cmFtYXQ=,size_16,color_FFFFFF,t_70)


5.字体颜色更换
1)NoteEditor中添加如下代码:
```
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    // Handle all of the possible menu actions.
    switch (item.getItemId()) {
        case R.id.menu_save:
            String text = mText.getText().toString();
            updateNote(text, null);
            finish();
            break;
        case R.id.menu_delete:
            deleteNote();
            finish();
            break;
        case R.id.menu_revert:
            cancelNote();
            break;
        case R.id.red_font:
            mText.setTextColor(Color.RED);
            break;
        case R.id.black_font:
            mText.setTextColor(Color.BLACK);
        case R.id.font_10:
            mText.setTextSize(20);
            break;
        case R.id.font_16:
            mText.setTextSize(32);
            break;
        case R.id.font_20:
            mText.setTextSize(40);
            break;
    }
    return super.onOptionsItemSelected(item);
}
```
2)editor_options_menu.xml中添加如下代码:
```
<item
    android:title="字体颜色">
    <menu>
        <group>
            <item android:id="@+id/red_font"
                android:title="红色"/>
            <item android:id="@+id/black_font"
                android:title="黑色"/>
        </group>
    </menu>
</item>
```

截图如下所示:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190519143227437.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NhbW11cmFtYXQ=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190519143235818.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NhbW11cmFtYXQ=,size_16,color_FFFFFF,t_70)

6.导出功能：
1)在AndroidManifest.xml中加入权限，定义样式：
```
<uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"/>
<!-- 向SD卡写入数据权限 -->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<activity android:name="OutputText"
    android:label="@string/output_name"
    android:theme="@android:style/Theme.Holo.Dialog"
    android:windowSoftInputMode="stateVisible">
</activity>
```

2)editor_options_menu.xml中添加一个选项
```
<item android:id="@+id/menu_output"
    android:title="@string/menu_output" />
  ```
3)NoteEditor中添加一个新的函数
```
private final void outputNote() {
    Intent intent = new Intent(null,mUri);
    intent.setClass(NoteEditor.this,OutputText.class);
    NoteEditor.this.startActivity(intent);
}
```

4)output_text.xml是我新建的导出文件界面的布局
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:paddingLeft="6dip"
    android:paddingRight="6dip"
    android:paddingBottom="3dip">
    <EditText android:id="@+id/output_name"
        android:maxLines="1"
        android:layout_marginTop="2dp"
        android:layout_marginBottom="15dp"
        android:layout_width="wrap_content"
        android:ems="25"
        android:layout_height="wrap_content"
        android:autoText="true"
        android:capitalize="sentences"
        android:scrollHorizontally="true" />
    <Button android:id="@+id/output_ok"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="right"
        android:text="@string/output_ok"
        android:onClick="OutputOk" />
</LinearLayout>
```
5)并新建OutputText.java，关键代码如下：
```
    private void write()
    {
        try
        {

            if (Environment.getExternalStorageState().equals(
                    Environment.MEDIA_MOUNTED)) {
            
                File sdCardDir = Environment.getExternalStorageDirectory();             
                File targetFile = new File(sdCardDir.getCanonicalPath() + "/" + mName.getText() + ".txt");              
                PrintWriter ps = new PrintWriter(new OutputStreamWriter(new FileOutputStream(targetFile), "UTF-8"));
                ps.println(TITLE);
                ps.println(NOTE);
                ps.println("创建时间：" + CREATE_DATE);
                ps.println("最后一次修改时间：" + MODIFICATION_DATE);
                ps.close();
                Toast.makeText(this, "保存成功,保存位置：" + sdCardDir.getCanonicalPath() + "/" + mName.getText() + ".txt", Toast.LENGTH_LONG).show();
            }
        }
        catch (Exception e)
        {
            e.printStackTrace();
        }
    }

}
```
截图如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190519144244614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NhbW11cmFtYXQ=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190519144254628.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NhbW11cmFtYXQ=,size_16,color_FFFFFF,t_70)


7.排序功能
1)list_options_menu.xml中添加:
```
<item
    android:id="@+id/menu_sort"
    android:title="@string/menu_sort"
    android:icon="@android:drawable/ic_menu_sort_by_size"
    app:showAsAction="always" >
    <menu>
        <item
            android:id="@+id/menu_sort1"
            android:title="@string/menu_sort1"/>
        <item
            android:id="@+id/menu_sort2"
            android:title="@string/menu_sort2"/>

    </menu>
</item>
```
2)NoteList中增加两个选项
```
case R.id.menu_sort1:
        cursor = managedQuery(
                getIntent().getData(),            
                PROJECTION,                      
                null,                          
                null,                          
                NotePad.Notes._ID 
                );
        adapter = new MyCursorAdapter(
                this,
                R.layout.noteslist_item,
                cursor,
                dataColumns,
                viewIDs
        );
        setListAdapter(adapter);
        return true;

    case R.id.menu_sort2:
        cursor = managedQuery(
                getIntent().getData(),          
                PROJECTION,                      
                null,                            
                null,                       
                NotePad.Notes.DEFAULT_SORT_ORDER 
        );
        adapter = new MyCursorAdapter(
                this,
                R.layout.noteslist_item,
                cursor,
                dataColumns,
                viewIDs
        );
        setListAdapter(adapter);
```
截图如下:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190519144729850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1NhbW11cmFtYXQ=,size_16,color_FFFFFF,t_70)













































