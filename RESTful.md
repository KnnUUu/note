安全性：不会改变服务器状态  
冪等性：相同命令不管执行几次结果都一样

|   |作用|安全性|冪等性|   |
|---|---|---|---|---|
|GET|获取数据|⭕|⭕|   |
|POST|往服务器追加或者更新数据|❌|❌|   |
|PUT|与POST类似但有冪等性|❌|⭕|   |
|DELETE|   |   |   |   |