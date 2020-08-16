# Tips and Tricks:

## Output SQL Query and timing:

To output to the screen the last queries ran you can use this:

```
DB::enableQueryLog(); // Enable query log

// Your Eloquent query executed by using get()
    $chartData = Transaction::query()
        ->where('merchant_code', $this->user->m_code)
        ->where('created_at', '>=', $monthBeginning)
        ->select([DB::raw("count(*) as count"), 'created_at'])
        ->orderBy('created_at')
        ->groupBy(DB::raw('day(created_at)'))
        ->get()
        ->toArray();
        
dd(DB::getQueryLog()); // Show results of log
```
Run:
```
array:1 [▼
  0 => array:3 [▼
    "query" => "select count(*) as count, `created_at` from `transactions` where `merchant_code` = ? and `created_at` >= ? and `transactions`.`deleted_at` is null group by day(created_at) order by `created_at` asc ◀"
    "bindings" => array:2 [▼
      0 => "dev1212"
      1 => "2020-08-01 00:00:00"
    ]
    "time" => 0.82
  ]
]
```

To get transactions count during this month, you must select count and use groupBy, and use
```
->select([DB::raw("count(*) as count"), 'created_at'])
->groupBy(DB::raw('day(created_at)'))
```
```
$chartData = Transaction::query()
    ->where('merchant_code', $this->user->m_code)
    ->where('created_at', '>=', $monthBeginning)
    ->select([DB::raw("count(*) as count"), 'created_at'])
    ->orderBy('created_at')
    ->groupBy(DB::raw('day(created_at)'))
    ->get()
    ->toArray();
```