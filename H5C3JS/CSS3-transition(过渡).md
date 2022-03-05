## CSS3-transition(过渡)

| 编号 | 属性                       | 描述                               |
| :--: | :------------------------- | :--------------------------------- |
|  1   | transition                 | 简写的属性可调整编号2，3，4，5     |
|  2   | transition-property        | 过渡的属性的名称                   |
|  3   | transition-duration        | 过渡持续的时间，简写属性值默认为 0 |
|  4   | transition-timing-function | 过渡的时间曲线，默认是 "ease"      |
|  5   | transition-delay           | 过渡n秒开始，默认n=0               |

| transition-timing-function: 值; | 描述                           |
| :------------------------------ | :----------------------------- |
| linear                          | 均速                           |
| ease                            | 先慢后快                       |
| ease-in                         | 开始时慢速                     |
| ease-out                        | 结束时慢速                     |
| ease-in-out                     | 开始和结束时慢速               |
| cubic-bezier(*n*,*n*,*n*,*n*)   | 贝塞尔曲线（每个n值都是(0,1)） |