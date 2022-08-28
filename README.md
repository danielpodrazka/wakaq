# WakaQ
Distributed background task queue for Python backed by Redis, a super minimal Celery.

## Features

* Queue priority
* Delayed tasks (run tasks after a timedelta eta)
* Scheduled periodic tasks
* [Broadcast][broadcast] a task to all workers
* Task [soft][soft timeout] and [hard][hard timeout] timeout limits
* Super minimal

Want more features like rate limiting, task deduplication, etc? Too bad, feature PRs are not accepted. Maximal features belong in your app’s worker tasks.

## ToDo

* [x] scheduler
* [x] admin info inspection, purging queues, etc
* [x] broadcast task
* [x] handle child process crash/exception and re-fork
* [x] timeouts
* [ ] logging
* [ ] signals
* [ ] pre_fork(parent only) and post_fork(children only) hooks/signals

## Example

```python
from wakaq import WakaQ


wakaq = WakaQ(
    queues=[
        (0, 'a-high-priority-queue'),
        (1, 'a-medium-priority-queue'),
        (2, 'a-low-priority-queue'),
    ],
)


@wakaq.task(queue='medium-priority-queue')
def mytask(x, y):
    print(x + y)


if __name__ == '__main__':
    # add 1 plus 1 on a worker somewhere, overwriting the default queue from medium to high
    mytask.delay(1, 1, queue='high-priority-queue')
```


[broadcast]: https://github.com/wakatime/wakaq/blob/804a4afd6c66a9eafa0bdbe7e49d7484079cb25e/wakaq/task.py#L44
[soft timeout]: https://github.com/wakatime/wakaq/blob/804a4afd6c66a9eafa0bdbe7e49d7484079cb25e/wakaq/exceptions.py#L4
[hard timeout]: https://github.com/wakatime/wakaq/blob/804a4afd6c66a9eafa0bdbe7e49d7484079cb25e/wakaq/worker.py#L199
