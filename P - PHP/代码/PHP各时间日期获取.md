PHP中各种时间点的时间戳获取方式
```php
// 当天 零点
$curr_day_begin = strtotime(date("Y-m-d 00:00:00", time()));
// 当前 23点59分59秒
$curr_day_end = strtotime(date("Y-m-d 23:59:59", time()));

// 昨天 零点
$before_day_begin = strtotime(date("Y-m-d 00:00:00", strtotime("-1 day",time())));
$before_day_end = strtotime(date("Y-m-d 23:59:59", strtotime("-1 day",time())));

// 近7天（不好含当天） 零点
$sevent_day_begin = strtotime(date("Y-m-d 00:00:00",strtotime("-7 day")));
// 近7天（不好含当天） 23点59分59秒
$sevent_day_end = strtotime(date("Y-m-d 23:59:59", strtotime("-7 day")));

// 本周一 零点
$curr_week_begin = strtotime(date("Y-m-d 00:00:00", strtotime("this week Monday", time())));
// 本周日 23点59分59秒
$curr_week_end = strtotime(date("Y-m-d 23:59:59", strtotime("this week Sunday", time())));

// 本月月初
$curr_month_begin = strtotime(date("Y-m-01 00:00:00", time()));
// 本月月末
$curr_month_end = strtotime(date("Y-m-d 23:59:59", strtotime("+1 month -1 day",$curr_month_begin)));

// 上月月初
$before_month_begin = strtotime(date("Y-m-d 00:00:00",strtotime("{$curr_month_begin} -1 month")));
// 上月月末
$defore_month_end = strtotime(date("Y-m-d 23:59:59", strtotime("{$curr_month_begin} -1 day")));
```
