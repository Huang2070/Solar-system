```java
//同时开始: 多个子线程同时开启, 等待某个条件完成后, 子线程同时向后执行
@Test
public void test193() throws InterruptedException {
    String key = RandomStringUtils.randomAlphabetic(5);
    CountDownLatch countDownLatch = new CountDownLatch(50);
    for (int i = 0; i < 50; i++) {
        final int j = i;
        new Thread(() -> {
            try {
                countDownLatch.await();
                Thread.sleep(j);
            }
            catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println("线程" + j + "执行!");
        }).start();
        countDownLatch.countDown();
    }
    Thread.sleep(100000000);
}
```

```java
//同时结束: 主线程等待子线程执行完成后, 继续执行主线程
/Users/hj_2021/private/Solar-system/Java技术/多线程/CountDownLatchTest.java
```