# QueryTasks

A basic query to get task in the system. 

You have two endpoint to query task, `/tasks` and `/tasks/<task_id>`. 

almost all response query `/tasks` follows this format:

```python
{
    history: [TASKOBJ],
    current: [TASKOBJ],
    pending: [TASKOBJ]
}
```
> the length of current list will not exceed 1, and the current task object will contain an extra `preview` attribute.

`/tasks/<task_id>` will return a single task object.

**Get all tasks**

`GET /tasks`

**Get a single task**

`GET /tasks/<task_id>`

**Get history/current/pending tasks**

`GET /tasks?query=history|current|pending`

**Specifying the page and page size**

`GET /tasks?query=history&page=1&page_size=10`

**Filtering tasks**

`GET /tasks?query=history&start_at=2024-07-30T11:20:22&end_at=2024-07-31T11:20:22`

**Delete tasks**

`GET /tasks?query=history&start_at=2024-07-30T11:20:22&end_at=2024-07-31T11:20:22&action=delete`
 