## Worker的封装

#### 1.Worker的有参构造

```java
Runnable firstTask;
Worker(Runnable firstTask) {
    setState(-1); // inhibit interrupts until runWorker
    this.firstTask = firstTask;
    this.thread = getThreadFactory().newThread(this);
}
  /** Delegates main run loop to outer runWorker  */
    public void run() {
         runWorker(this);
}
```

```java
 final void runWorker(Worker w) {
 		//获取当前线程
        Thread wt = Thread.currentThread();
     	//获取线程中的任务
        Runnable task = w.firstTask;
		w.firstTask = null;
        w.unlock(); // allow interrupts
		//标识为true
        boolean completedAbruptly = true;
        try {
            //如果任务不为空,执行任务。/如果任务为空,从阻塞队列中获取任务,执行任务
            while (task != null || (task = getTask()) != null) {
                w.lock();
               
                //获取当前状态,比较当前状态是否大于等于STOP,线程中断!
                if ((runStateAtLeast(ctl.get(), STOP) ||
                     (Thread.interrupted() &&
                      runStateAtLeast(ctl.get(), STOP))) &&
                    !wt.isInterrupted())
                    wt.interrupt();
                try {
                    //执行任务之前的操作
                    beforeExecute(wt, task);
                    Throwable thrown = null;
                    try {
                        //开始执行任务
                        task.run();
                    } catch (RuntimeException x) {
                        thrown = x; throw x;
                    } catch (Error x) {
                        thrown = x; throw x;
                    } catch (Throwable x) {
                        thrown = x; throw new Error(x);
                    } finally {
                        afterExecute(task, thrown);
                    }
                } finally {
                    task = null;
                    w.completedTasks++;
                    w.unlock();
                }
            }
            completedAbruptly = false;
        } finally {
            //线程执行完毕后做的事情
            processWorkerExit(w, completedAbruptly);
        }
    }

```

getTask()阻塞

```java
 private Runnable getTask() {
        boolean timedOut = false; // Did the last poll() time out?

        for (;;) {
            int c = ctl.get();
            int rs = runStateOf(c);

            // Check if queue empty only if necessary.
            if (rs >= SHUTDOWN && (rs >= STOP || workQueue.isEmpty())) {
                decrementWorkerCount();
                return null;
            }

            int wc = workerCountOf(c);

            // Are workers subject to culling?
            boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;

            if ((wc > maximumPoolSize || (timed && timedOut))
                && (wc > 1 || workQueue.isEmpty())) {
                if (compareAndDecrementWorkerCount(c))
                    return null;
                continue;
            }

            try {
                Runnable r = timed ?
                    workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :
                    //阻塞
                    workQueue.take();
                if (r != null)
                    return r;
                timedOut = true;
            } catch (InterruptedException retry) {
                timedOut = false;
            }
        }
    }
```

