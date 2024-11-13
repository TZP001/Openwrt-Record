## windows使用心得[【项目地址】](https://github.com/dockur/windows)
#### 安装
 ```
docker run -it \
      --name=windows \
      --rm \
      -p 8006:8006 \
      --device=/dev/kvm \
      --cap-add NET_ADMIN \
      --stop-timeout 120 \
      -v <你的本地目录>/win:/storage
      -e TZ="Asia/Shanghai" \
      -e VERSION=10 \
      -e DISK_SIZE=10G \
      -e RAM_SIZE=1G \
      --dns 223.5.5.5 \
      dockurr/windows
```
------------------
### 注意事项
* 更多说明参考[这里](https://github.com/dockur/windows/blob/master/readme.md)
* ```RAM_SIZE```要放在最后，不然可能会设置失败，导致内存过大无法使用
* -v 挂载目录是你与外界传递文件的渠道，除非你的cpu很强，否则一定要用
* ```VERSION```版本选择
  | **Value** | **Version**           | **Size** |
  |---|---|---|
  | `11`   | Windows 11 Pro           | 5.4 GB   |
  | `11l`  | Windows 11 LTSC          | 4.2 GB   |
  | `11e`  | Windows 11 Enterprise    | 5.8 GB   |
  ||||
  | `10`   | Windows 10 Pro           | 5.7 GB   |
  | `10l`  | Windows 10 LTSC          | 4.6 GB   |
  | `10e`  | Windows 10 Enterprise    | 5.2 GB   |
  ||||
  | `8`    | Windows 8.1 Pro          | 4.0 GB   |
  | `8e`   | Windows 8.1 Enterprise   | 3.7 GB   |
  | `7e`   | Windows 7 Enterprise     | 3.0 GB   |
  | `ve`   | Windows Vista Enterprise | 3.0 GB   |
  | `xp`   | Windows XP Professional  | 0.6 GB   |
  ||||
  | `2025` | Windows Server 2025      | 5.0 GB   |
  | `2022` | Windows Server 2022      | 4.7 GB   |
  | `2019` | Windows Server 2019      | 5.3 GB   |
  | `2016` | Windows Server 2016      | 6.5 GB   |
  | `2012` | Windows Server 2012      | 4.3 GB   |
  | `2008` | Windows Server 2008      | 3.0 GB   |
  | `2003` | Windows Server 2003      | 0.6 GB   |
