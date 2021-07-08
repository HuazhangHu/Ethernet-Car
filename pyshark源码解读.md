## pyshark源码

###  一、capture模块

1. 基类 capture

   重要参数:

   ​	 `only_summaries` 当等于True时，仅产生数据摘要，速度快。展示固定的字段

   ​		比较有用的字段包括：

   ```
   'delta', 'destination', 'info', 'ip id', 'length', 'no', 'protocol', 'source', 'stream', 'summary_line', 'time', 'window'
   ```

   ​	`keep_packets` 用于在调用next()函数或者遍历时，是否保存之前读取的数据包，用于节省内存 

   `display_filter` display过滤器，例如`display_filter=dns` 时只显示最高层是DNS的包 

   `bpf_filter` BPF过滤器，例如`bpf_filter='ip and tcp port 80'` 只获取相应条件的包

   重要变量： 

    	self._packets=[] 内部存储包的列表

   `load_packets(packet_count=0,timeout=None)`方法

   ```
   load_packets(packet_count=0,timeout=None)
   #从数据源（包、网卡等）中读取包并加如入内部列表中
   #param packet_count：加入列表的包的数量，默认0表示读取至全部加载
   #param timeout:读取的限定时间
   ```

   `clear()方法` 清空列表 `close()` 方法关闭capture对象

   `set_debug()` 方法 将capture 变成debug模式

   `next()` 方法用于遍历至下一个包

   `apply_on_packets()` 方法 接受一个函数作为参数并将其作用于所有数据包

   ```
   def print_highest_layer(pkt):
   	print(pkt.highest_layer)
   cap = pyshark.FileCapture('test.pcap', keep_packets=False)
   cap.apply_on_packets(print_highest_layer)
   ```

   ### 二、packet模块

   该模块主要针对抓下来的包执行

   `layers` 参数显示包的层级结构

   `highest_layer` 显示最高层的协议

   

