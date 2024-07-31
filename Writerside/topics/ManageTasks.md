# QueryTasks

一个基本的任务查询系统

一共有两个接口 `/tasks` 和 `/tasks/<task_id>`. 

基本上所有请求 `/tasks` 的返回格式差不多都是下面的格式:

```python
{
    history: [TASKOBJ],
    current: [TASKOBJ],
    pending: [TASKOBJ]
}
```
> 当前任务列表最多只有 1 个元素，而且当前任务会多一个 `preview` 属性

`/tasks/<task_id>` 返回单个任务的详细信息

**获取所有任务**

`GET /tasks`

**获取单个任务**

`GET /tasks/<task_id>`

**获取 历史/当前/等待 任务**

`GET /tasks?query=history|current|pending`

**指定页码及页面大小**

`GET /tasks?query=history&page=1&page_size=10`

**筛选任务**

`GET /tasks?query=history&start_at=2024-07-30T11:20:22&end_at=2024-07-31T11:20:22`

**删除任务**

`GET /tasks?query=history&start_at=2024-07-30T11:20:22&end_at=2024-07-31T11:20:22&action=delete`
 