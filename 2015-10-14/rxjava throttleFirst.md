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
通过throttleFirst可以简单的解决掉手抖点击多次打开重复界面的问题。



> Written with [StackEdit](https://stackedit.io/).