# lab3

一、Android ListView的用法
public class MainActivity extends AppCompatActivity {

    private String[] name = new String[]{"Lion", "Tiger", "Monkey", "Dog", "Cat", "Elephant"};
    private int[] touxiang = new int[]{R.drawable.lion, R.drawable.tiger, R.drawable.monkey, R.drawable.dog,
                                        R.drawable.cat, R.drawable.elephant};

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        List<Map<String, Object>> listItems = new ArrayList<>();
        for(int i = 0; i < name.length; i++){
            Map<String, Object> listItem = new HashMap<>();
            listItem.put("name", name[i]);
            listItem.put("touxiang", touxiang[i]);
            listItems.add(listItem);
        }
        SimpleAdapter simpleAdapter = new SimpleAdapter(this, listItems, R.layout.simple_item,
                new String[]{"name", "touxiang"}, new int[]{R.id.name, R.id.touxiang});
        ListView listview = findViewById(R.id.lv);
        listview.setAdapter(simpleAdapter);

        listview.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {

                Toast.makeText(MainActivity.this, name[position], Toast.LENGTH_SHORT).show();
            }
        });
    }
}
![image](https://user-images.githubusercontent.com/82015926/137663500-3ab29cb0-cf57-461e-970c-97e96464019f.png)

二、创建自定义布局的AlertDialog
public class MainActivity2 extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main2);
        Button button = findViewById(R.id.btn);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                View dialogview =getLayoutInflater().inflate(R.layout.dialog_view, null);

                AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity2.this);
                builder.setView(dialogview)
                .setPositiveButton("Sign in", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        Toast.makeText(MainActivity2.this, "点击了登录按钮", Toast.LENGTH_SHORT).show();
                    }
                })
                .setNegativeButton("Cancel", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        Toast.makeText(MainActivity2.this, "点击了取消按钮", Toast.LENGTH_SHORT).show();
                    }
                })
                .create()
                .show();
            }
        });
    }
}
xml:
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <ImageView
        android:id="@+id/iv"
        android:layout_width="310dp"
        android:layout_height="65dp"
        android:src="@drawable/header_logo"/>

    <EditText
        android:id="@+id/et1"
        android:padding="10dp"
        android:hint="@string/username"
        android:layout_width="310dp"
        android:layout_height="50dp"/>

    <EditText
        android:padding="10dp"
        android:id="@+id/rt2"
        android:inputType="textPassword"
        android:hint="@string/password"
        android:layout_width="310dp"
        android:layout_height="50dp"/>

</LinearLayout>
![image](https://user-images.githubusercontent.com/82015926/137663669-861a4a89-eb4c-4509-a296-abf233151f7a.png)

三、使用XML定义菜单
public class MainActivity3 extends AppCompatActivity {

    private static final int FONT_10 = 0x111;
    private static final int FONT_16 = 0x112;
    private static final int FONT_20 = 0x113;
    private static final int ORDINARY = 0x114;
    private static final int FONT_RED = 0x115;
    private static final int FONT_BLACK = 0x116;
    private TextView textView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main3);
        textView = findViewById(R.id.tv);
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        SubMenu fontMenu = menu.addSubMenu("字体大小");
        fontMenu.setHeaderTitle("选择字体大小");
        fontMenu.add(0, FONT_10, 0, "10号字体");
        fontMenu.add(0, FONT_16, 0, "16号字体");
        fontMenu.add(0, FONT_20, 0, "20号字体");

        menu.add(0, ORDINARY, 0, "普通菜单项");

        SubMenu colorMenu = menu.addSubMenu("字体颜色");
        colorMenu.setHeaderTitle("选择字体颜色");
        colorMenu.add(0, FONT_RED, 0, "红色");
        colorMenu.add(0, FONT_BLACK, 0, "黑色");
        return super.onCreateOptionsMenu(menu);
    }

    @Override
    public boolean onOptionsItemSelected(@NonNull MenuItem item) {
        switch (item.getItemId()){
            case FONT_10:
                textView.setTextSize(10 * 2);
                break;
            case FONT_16:
                textView.setTextSize(16 * 2);
                break;
            case FONT_20:
                textView.setTextSize(20 * 2);
                break;
            case ORDINARY:
                Toast.makeText(MainActivity3.this, "点击了普通菜单项", Toast.LENGTH_SHORT).show();
                break;
            case FONT_RED:
                textView.setTextColor(Color.RED);
                break;
            case FONT_BLACK:
                textView.setTextColor(Color.BLACK);
                break;

        }
        return true;
    }
}
![image](https://user-images.githubusercontent.com/82015926/137664303-1c1b8242-b8d8-476c-83f6-3305be1ccde5.png)
![image](https://user-images.githubusercontent.com/82015926/137664323-d449c6c7-4bb4-447e-b01a-7936f34dc583.png)
![image](https://user-images.githubusercontent.com/82015926/137664352-3b76609e-2712-424d-b83e-cfab6580e209.png)

四、创建上下文操作模式(ActionMode)的上下文菜单
public class MainActivity4 extends AppCompatActivity {

    private String[] order = new String[]{"One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "ten"};
    TextView count;
    View actionBarView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main4);

        ListView listView = findViewById(R.id.lv);
        MyAdapter myAdapter = new MyAdapter(Arrays.asList(order), this, listView);
        listView.setAdapter(myAdapter);

        listView.setChoiceMode(ListView.CHOICE_MODE_MULTIPLE_MODAL);
        listView.setMultiChoiceModeListener(new AbsListView.MultiChoiceModeListener() {
            @Override
            public void onItemCheckedStateChanged(ActionMode mode, int position, long id, boolean checked) {
                updateSelectedCount();
                mode.invalidate();
                myAdapter.notifyDataSetChanged();
            }

            @Override
            public boolean onCreateActionMode(ActionMode mode, Menu menu) {
                mode.getMenuInflater().inflate(R.menu.menu, menu);
                if (actionBarView == null){
                    actionBarView = LayoutInflater.from(MainActivity4.this).inflate(R.layout.actionbar_view, null);
                    count = (TextView) actionBarView.findViewById(R.id.count);
                }
                mode.setCustomView(actionBarView);
                return true;
            }

            @Override
            public boolean onPrepareActionMode(ActionMode mode, Menu menu) {
                return false;
            }

            @Override
            public boolean onActionItemClicked(ActionMode mode, MenuItem item) {
                return false;
            }

            @Override
            public void onDestroyActionMode(ActionMode mode) {
                listView.clearChoices();
            }

            public void updateSelectedCount(){
                int selectedCount = listView.getCheckedItemCount();
                count.setText(selectedCount + " ");
            }
        });
    }
}
public class MyAdapter extends BaseAdapter {

    private List<String> data;
    private Context context;
    private ListView listView;

    public MyAdapter(List<String> data, Context context, ListView listView){
        this.data = data;
        this.context = context;
        this.listView = listView;
    }

    @Override
    public int getCount() {
        return data == null ? 0 : data.size();
    }

    @Override
    public Object getItem(int position) {
        return data.get(position);
    }

    @Override
    public long getItemId(int position) {
        return position;
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        ViewHolder viewHolder;
        if(convertView == null){
            viewHolder = new ViewHolder();
            convertView = LayoutInflater.from(context).inflate(R.layout.list_item, parent, false);
            viewHolder.iv = convertView.findViewById(R.id.iv);
            viewHolder.tv = convertView.findViewById(R.id.tv);
            convertView.setTag(viewHolder);
        }else{
            viewHolder = (ViewHolder) convertView.getTag();
        }
        viewHolder.iv.setImageResource(R.drawable.cat);
        viewHolder.tv.setText(data.get(position));
        if(listView.isItemChecked(position)){
            convertView.setBackgroundColor(Color.parseColor("#00BCD4"));
        }else{
            convertView.setBackgroundColor(Color.TRANSPARENT);
        }
        return convertView;
    }

    private final class ViewHolder{
        ImageView iv;
        TextView tv;
    }
}
xml：
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">

    <TextView
        android:id="@+id/count"
        android:textSize="20sp"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

    <TextView
        android:textSize="20sp"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/selected" />

</LinearLayout>

<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/delete"
        android:icon="@drawable/ic_baseline_delete_24"
        android:title="@string/mode1"
        app:showAsAction="always"/>

</menu>
![image](https://user-images.githubusercontent.com/82015926/137664774-05ef1ce6-c8b4-4c2b-a13f-fc5d334c91cd.png)
