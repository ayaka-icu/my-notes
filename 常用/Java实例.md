



```java
@Override
public List<TabProjectVo> queryListByIds(Long[] uuids) {
    
    List<TabProject> pros = baseMapper.selectList(new LambdaQueryWrapper<TabProject>()
        .in(TabProject::getUuid, Arrays.asList(uuids)));
    
    List<Long> ids = pros.stream().map(TabProject::getId).collect(Collectors.toList());
    
    List<TabJgysQy> qys = jgysQyService.selectByProjectIds(ids);
    
    Map<Long, List<TabJgysQy>> qyMap = qys.stream()
        .collect(Collectors.groupingBy(TabJgysQy::getProjectId));
    return pros.stream().map(pro -> {
        TabProjectVo vo = toBean(pro, TabProjectVo.class);
        List<TabJgysQy> projectQys = qyMap.getOrDefault(pro.getId(), Collections.emptyList());
        projectQys.forEach(qy -> vo.getJgysList().add(toBean(qy, TabJgysQyVo.class)));
        return vo;
    }).collect(Collectors.toList());
}
```



## 手动创建 阻塞队列

```java
package com.hmdp.tools;

import com.hmdp.entity.VoucherOrder;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

@Slf4j
@Component
public class BlockingQueueTest {

    //阻塞队列
    private BlockingQueue<VoucherOrder> orderTasks = new ArrayBlockingQueue<>(1024*1024);

    //线程池
    private static final ExecutorService SECKILL_ORDER_EXECUTOR = Executors.newSingleThreadExecutor();

    //执行阻塞队列
    @PostConstruct
    private void init(){
        SECKILL_ORDER_EXECUTOR.submit(new VoucherOrderHandle());
    }
    
    //处理订单的异步任务
    private class VoucherOrderHandle implements Runnable{
        @Override
        public void run() {
            //等待任务
            while(true){
                try {
                    //从阻塞队列中取
                    VoucherOrder voucherOrder = orderTasks.take();
                    //处理订单
                    //handleVoucherOrder(voucherOrder);
                } catch (Exception e) {
                    log.debug("订单任务失败", e);
                }
            }
        }
    }
}
```





## 单线程池 和 CountDownLatch

```java
package com.hmdp;

import com.hmdp.utils.RedisIdWorker;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.ContextConfiguration;

import javax.annotation.Resource;
import java.time.LocalDateTime;
import java.time.ZoneOffset;
import java.util.concurrent.CountDownLatch;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

@SpringBootTest
@ContextConfiguration(classes = MQApp.class)
class RedisIdWorkerTests {

    @Resource
    private RedisIdWorker redisIdWorker;

    private ExecutorService es= Executors.newFixedThreadPool(500);

    @Test
    void gitIdTest() throws InterruptedException {

        CountDownLatch countDownLatch = new CountDownLatch(300);

        //开始时间戳
        long startTimestamp = LocalDateTime.now().toEpochSecond(ZoneOffset.UTC);

        Runnable task = ()->{
            //一个任务生成100个id
            for (int i = 0; i < 100; i++) {
                long id = redisIdWorker.nextId("order");
                System.out.println(id);
            }
            countDownLatch.countDown();
        };

        //300个任务
        for (int i = 0; i < 300; i++) {
            es.submit(task);
        }

        countDownLatch.await();
        long stopTimestamp = LocalDateTime.now().toEpochSecond(ZoneOffset.UTC);
        System.out.println(stopTimestamp - startTimestamp);
    }


}
```

