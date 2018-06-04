# NotePadMay
基本要求和附加功能  （给分点）请直接跳转到    **三、功能演示** 
## 一.基本功能介绍：
### 1.时间戳

<img src=https://ws1.sinaimg.cn/large/006dRdovgy1frz5096v9vj30u01hc14j.jpg width="480px" align=center/>

### 2.便签搜索（根据标题及内容模糊搜索）

<img src=https://ws1.sinaimg.cn/large/006dRdovgy1frz54ss4xgj30u01hcwqf.jpg width="480px" align=center/>


## 二.技术分析：
### 1.时间戳分析：
因为数据库中已有时间字段，所以只需要格式化时间存入即可
NoteEditor.updateNote()函数中修改
        SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//设置日期格式</br>
        values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, df.format(new Date()));
 ### 2.便签搜索
   private void addSearchView() 
   {
        //给listview添加头部(search)

        View v=View.inflate(this, R.layout.notelistheader,null);
        getListView().addHeaderView(v);
        //给搜索框添加搜索功能
        final EditText et_Search=(EditText)v.findViewById(R.id.et_search);
        et_Search.addTextChangedListener(new TextWatcherForSearch(){
            @Override
            public void onTextChanged(CharSequence charSequence, int i, int i1, int i2) {
                super.onTextChanged(charSequence, i, i1, i2);
                if (charSequence.length()!=0 && et_Search.getText().toString().length()!=0){
                    String str_Search = et_Search.getText().toString();
                    Cursor search_cursor = managedQuery(
                            getIntent().getData(),            // Use the default content URI for the provider.
                            PROJECTION,                       // Return the note ID and title for each note.
                            NotePad.Notes.COLUMN_NAME_TITLE+" like ?",     //模糊查询数据库
                            new String[]{"%"+str_Search+"%"}, //匹配字符串条件                            // No where clause, therefore no where column values.
                            NotePad.Notes.DEFAULT_SORT_ORDER  // Use the default sort order.
                    );
                    adapter.swapCursor(search_cursor);//用搜索结果的cursor刷新listview
    
                }else {
                    if (cursor!=null)//删除搜索框中的text后刷新listview
                    adapter.swapCursor(cursor);//刷新listview
                }
            }
        });
    }
 ### 3.改变背景颜色
    使用底部按钮弹出对话框
      private void setBackgroundTwo(){
        AlertDialog.Builder builder=new AlertDialog.Builder(NotesList.this);
        LayoutInflater inflater=getLayoutInflater();
        View view=inflater.inflate(R.layout.dialogcolor,null);
        LinearLayout linearLayout=(LinearLayout) view.findViewById(R.id.dialoglinear);
        final int[] colorArray={
                Color.parseColor("#707070"),//黑
                Color.parseColor("#fcf9a4"),//黄
                Color.parseColor("#abed65"),//绿
                Color.parseColor("#33a9e1"),//蓝
                Color.parseColor("#070707"),//黑
                Color.parseColor("#1cdaef"),//蓝绿
                Color.parseColor("#fa77ab"),//粉色
        };
        for (int i=0;i<7;i++){//动态创建imageview 用以显示不同颜色
            ImageView imageView=new ImageView(NotesList.this);
            imageView.setLayoutParams(new LinearLayout.LayoutParams(90,120));
            imageView.setBackgroundColor(colorArray[i]);
            final int finalI = i;
            imageView.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    NoteAttribute.snoteBackground=colorArray[finalI];
                    getListView().setBackgroundColor(NoteAttribute.snoteBackground);
                    getListView().setBackgroundColor(colorArray[finalI]);
                    appbackground
                            .edit()
                            .putInt(NoteAttribute.NOTEBACKGROUND,colorArray[finalI])
                            .apply();//使用轻量级存储sharepreference异步将颜色存储在本地
                }
            });
            linearLayout.addView(imageView);
        }
        builder.setView(view).create().show();
    
    }
## 三.功能演示
### 1>基本功能
#### 1.notepad的主界面</br>
包含了时间戳</br>

<img src=https://ws1.sinaimg.cn/large/006dRdovgy1frz5096v9vj30u01hc14j.jpg width="480px" align=center/>

#### 2.添加便签</br>
<img src=https://ws1.sinaimg.cn/large/006dRdovgy1frz524k5rnj30u01hcqjf.jpg width="480px" align=center/>

#### 3.点击搜索</br>
添加了搜索功能</br>
<img src=https://ws1.sinaimg.cn/large/006dRdovgy1frz54ss4xgj30u01hcwqf.jpg width="480px" align=center/>

### 2>扩展功能（加分点）
#### 1.UI 美化
- 使用 android 原生控件，针对移动端手机操作进行了优化，提升了用户使用体验。
- 同时加入了主题色、背景色调整，增加了用户的人性化体验。
</br>

<img src=https://ws1.sinaimg.cn/large/006dRdovgy1frz564hqbjj30u01hc47h.jpg width="480px" align=center/>

#### 1.改变背景色
点击屏幕下方按钮：点开有惊喜！
</br>
<img src=https://ws1.sinaimg.cn/large/006dRdovgy1frz564hqbjj30u01hc47h.jpg width="480px" align=center/>

改变后的界面
</br>
<img src=https://ws1.sinaimg.cn/large/006dRdovgy1frz56b2u8cj30u01hck36.jpg width="480px" align=center/>

#### 2.语音笔记
通过讯飞的接口，实现了语音识别记笔记
</br>
<img src=https://ws1.sinaimg.cn/large/006dRdovgy1frz56qqpocg30go0tn7wi.jpg width="480px" align=center/></br>
语音识别技术实现参考博客：

[http://www.andyqian.com/2016/06/20/android/android_voice(2)/](http://www.andyqian.com/2016/06/20/android/android_voice(2)/)
