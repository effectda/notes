**rxjava throttleFirst

```java
final BehaviorSubject<View> behaviorSubject = BehaviorSubject.create();



        findViewById(R.id.test).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                behaviorSubject.onNext(v);
            }
        });



        behaviorSubject.throttleFirst(500, TimeUnit.MILLISECONDS).subscribe(new Action1<View>() {
            @Override
            public void call(View view) {
                System.out.println("click--------");
            }
        });
```
通过throttleFirst可以简单的解决掉手抖短时间内点击多次导致打开多个相同界面的问题。
