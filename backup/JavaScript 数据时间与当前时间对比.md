代码实现的功能为：` 根据判断返回不同的字符串`

```javascript
formatDisplayTime(timestamp) {
				const now = new Date()
				const date = new Date(timestamp)
				
				// 转换为同一时区的日期字符串进行比较
				const today = new Date(now.getFullYear(), now.getMonth(), now.getDate()).getTime()
				const yesterday = today - 24 * 60 * 60 * 1000
				const targetDate = new Date(date.getFullYear(), date.getMonth(), date.getDate()).getTime()
				
				// 格式化具体时间
				const hours = date.getHours().toString().padStart(2, '0')
				const minutes = date.getMinutes().toString().padStart(2, '0')
				const timeStr = `${hours}:${minutes}`
				
				if (targetDate === today) {
					return `今天 ${timeStr}`
				} else if (targetDate === yesterday) {
					return `昨天 ${timeStr}`
				} else {
					const month = (date.getMonth() + 1).toString().padStart(2, '0')
					const day = date.getDate().toString().padStart(2, '0')
					
					// 判断是否是当前年份
					if (date.getFullYear() === now.getFullYear()) {
						return `${month}-${day} ${timeStr}`
					} else {
						return `${date.getFullYear()}-${month}-${day} ${timeStr}`
					}
				}
			}
```

调用

```
:time="formatDisplayTime(item.createTime)"
```

绑定并传入时间与当前时间对比

| 时间                 | 显示                    |
| -------------------- | ----------------------- |
| 当天                 | 今天 xx:xx              |
| 昨天                 | 昨天 xx:xx              |
| 今年（非今天与昨天） | 月：日                  |
| 非今年               | 原样不动 保持最完整数据 |

