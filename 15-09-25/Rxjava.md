```java
package com.gqp.common.app;

import android.support.v7.app.ActionBarActivity;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.Menu;
import android.view.MenuItem;
import android.widget.Toast;
import com.gqp.common.executor.JobExecutor;
import com.gqp.common.rx.android.schedulers.AndroidSchedulers;
import rx.Observable;
import rx.Subscriber;
import rx.schedulers.Schedulers;
import rx.subscriptions.CompositeSubscription;

import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;
import java.util.concurrent.TimeUnit;


public class MainActivity extends AppCompatActivity {

    private CompositeSubscription compositeSubscription = new CompositeSubscription();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Callable<String> stringCallable = new Callable<String>() {
            @Override
            public String call() throws Exception {
                Thread.sleep(10001);

                System.out.println("call");

                return "xxxxxxxxxxxx";
            }
        };

        FutureTask<String> futureTask = new FutureTask<>(stringCallable);

        JobExecutor.getInstance().execute(futureTask);

        compositeSubscription.add(Observable.from(futureTask).observeOn(AndroidSchedulers.mainThread()).subscribeOn(Schedulers.from(JobExecutor.getInstance())).subscribe(new Subscriber<String>() {
            @Override
            public void onCompleted() {

            }

            @Override
            public void onError(Throwable e) {

                e.printStackTrace();

                Toast.makeText(MainActivity.this, "error", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onNext(String s) {
                Toast.makeText(MainActivity.this, s, Toast.LENGTH_SHORT).show();
            }
        }));

    }

//    private static Callable<String> stringCallable = new Callable<String>() {
//        @Override
//        public String call() throws Exception {
//            Thread.sleep(10001);
//
//            System.out.println("call");
//
//            return "xxxxxxxxxxxx";
//        }
//    };
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }

        return super.onOptionsItemSelected(item);
    }


    @Override
    public void finish() {
        super.finish();

        System.out.println("finish:"+System.currentTimeMillis());
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        compositeSubscription.unsubscribe();
        compositeSubscription=null;
        System.out.println("onDestroy:"+System.currentTimeMillis());
        App.getRefWatcher(this).watch(this);

    }
}

```