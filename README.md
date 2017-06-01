# AdapterBasic
ListView RecyclerView 간단기는응용
```java
package com.guozhe.android.adapterbasic;

import android.content.Intent;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;

import java.util.ArrayList;

public class ListActivity extends AppCompatActivity {
    ListView listView;
    ArrayList<String> datas = new ArrayList<>();

    // 다른 액티비티와 데이터를 주고받을때 사용하는 키를 먼저 정의해둔다
    public static final String DATA_KEY = "ListActivityData";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list);

        listView = (ListView) findViewById(R.id.listView);
        // 1. 데이터
        for(int i=0; i<100 ; i++){
            datas.add("데이터"+i);
        }
        // 2. 아답터
        ArrayAdapter adapter = new ArrayAdapter(this, android.R.layout.simple_list_item_1,datas);

        // 3. 뷰 > 연결 < 아답터
        listView.setAdapter(adapter);

        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                // Activity 에 값 전달하기
                // 1. 전달받을 목적지 Intent 생성
                Intent intent = new Intent(ListActivity.this, DetailActivity.class);
                // 2. putExtra 로 값 입력
                intent.putExtra(DATA_KEY, datas.get(position));
                // 3. intent 를 이용한 Activity 생성 요청
                startActivity(intent);
            }
        });
    }
}
package com.guozhe.android.adapterbasic;

import android.content.Intent;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.TextView;

public class DetailActivity extends AppCompatActivity {

    TextView textView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_detail);

        textView = (TextView) findViewById(R.id.textView);

        // Activity 에서 넘어온 값 처리하기
        // 1. intent 를 꺼낸다
        Intent intent = getIntent();
        // 2. 값의 묶음인 bundle 을 꺼낸다
        Bundle bundle = intent.getExtras(); // 데이터의 모음
        String result = "";
        // 3. bundle 이 있는지 유효성 검사를 한다.
        if(bundle != null) {
            // 3.1 bundle 이 있으면 값을 꺼내서 변수에 담는다
            result = bundle.getString(ListActivity.DATA_KEY);
        }

        textView.setText(result);

        findViewById(R.id.btnBack).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                finish();
            }
        });
    }
}
```
