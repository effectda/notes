**rxjava take操作符与interval用法**

```java
subscription = Observable.interval(1, TimeUnit.SECONDS)
                .observeOn(AndroidSchedulers.mainThread())
                .subscribeOn(Schedulers.io())
                .take(20)
                .subscribe(new Action1<Long>() {
                    @Override
                    public void call(Long aLong) {
                        System.out.println(aLong);

                    }
                });
```
每隔1s会调用一次System.out.println(aLong);一共调用20次；可以用于倒计时类似的场景。subscription.unsubscribe();后会停止。
