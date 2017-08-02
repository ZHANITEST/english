# std.parallelism

편리한 다중 프로그래밍이 필요하시다면
고수준의 `std.parallelism`모듈이 적당합니다.

### 병렬(parallel)

[`std.parallelism.parallel`](http://dlang.org/phobos/std_parallelism.html#.parallel)함수는 `foreach`문의 처리부분을 서로 다른 스레드로 알아서 나눠줍니다:

    // arr의 병렬분리
    auto arr = iota(1,100).array;
    foreach(ref i; parallel(arr)) {
        i = i*i;
    }

`parallel`함수는 내부적으로 `opApply`연산자를 이용합니다.

The global `parallel`  `taskPool.parallel`의 단축인 `parallel`은
which is a `TaskPool` that 작업자 스레드는 *총 cpus - 1*를 사용하는 태스크풀은 
working threads.


`taskPool.parallel`함수명을 짥게 줄인 `parallel`은
`TaskPool`의 *CPU의(코어) 개수 -1* 갯수로 스레드를 생성합니다.
생성한 인스턴스는 병렬적인 수준의 제어를 허용합니다.

`parallel` uses the `opApply` operator internally.
The global `parallel`  is a shortcut to `taskPool.parallel`
which is a `TaskPool` that uses *total number of cpus - 1*
working threads. Creating your own instance allows
to control the degree of parallelism.

여기서 조심해야할 것은 `parallel`의 본문 열거는 반드시 확실하게 해야합니다.
다른 작업 유닛에서 접근하는 요소응
Beware that the body of a `parallel` iteration must
make sure that it doesn't modify items that another
working unit might have access to.

`workingUnitSize`는 작업자 스레드 당 처리할 요소의 개수를 
The optional `workingUnitSize` specifies the number of elements processed
per worker thread.

### reduce

The function
[`std.algorithm.iteration.reduce`](http://dlang.org/phobos/std_algorithm_iteration.html#reduce) -
known from other functional contexts as *accumulate* or *foldl* -
calls a function `fun(acc, x)` for each element `x`
where `acc` is the previous result:

    // 0 is the "seed"
    auto sum = reduce!"a + b"(0, elements);

[`Taskpool.reduce`](http://dlang.org/phobos/std_parallelism.html#.TaskPool.reduce)
is the parallel analog to `reduce`:

    // Find the sum of a range in parallel, using the first
    // element of each work unit as the seed.
    auto sum = taskPool.reduce!"a + b"(nums);

`TaskPool.reduce` splits the range into
sub ranges that are reduced in parallel. Once these
results have been calculated, the results are reduced
themselves.

### `task()`

[`task`](http://dlang.org/phobos/std_parallelism.html#.task) is a wrapper for a function
that might take longer or should be executed in
its own working thread. It can either be enqueued
in a taskpool:

    auto t = task!read("foo.txt");
    taskPool.put(t);

Or directly be executed in its own, new thread:

    t.executeInNewThread();

To get a task's result call `yieldForce`
on it. It will block until the result is available.

    auto fileData = t.yieldForce;

### In-depth

- [Parallelism in _Programming in D_](http://ddili.org/ders/d.en/parallelism.html)
- [std.parallelism](http://dlang.org/phobos/std_parallelism.html)

## {SourceCode}

```d
import std.parallelism;
import std.array: array;
import std.stdio: writeln;
import std.range: iota;

string theTask()
{
    import core.thread;
    Thread.sleep( dur!("seconds")(1) );
    return "Hello World";
}

void main()
{
    // taskpool with two threads
    auto myTaskPool = new TaskPool(2);
    // Stopping the task pool is important!
    scope(exit) myTaskPool.stop();

    // Start long running task
    // and do some other stuff in the mean
    // time..
    auto task = task!theTask;
    myTaskPool.put(task);

    auto arr = iota(1, 10).array;
    foreach(ref i; myTaskPool.parallel(arr)) {
        i = i*i;
    }

    writeln(arr);

    import std.algorithm: map;

    // Use reduce to calculate the sum
    // of all squares in parallel.
    auto result = taskPool.reduce!"a+b"(
        0.0, iota(100).map!"a*a");
    writeln("Sum of squares: ", result);

    // Get our result we sent to background
    // earlier.
    writeln(task.yieldForce);
}
```
