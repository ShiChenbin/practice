# 前言
记录实际开发经过，并对不懂的知识做总结
# readme
公司项目关于华东某项目的开发，该文章不涉及公司利益，仅对实习过程中的知识做一个记录

- 系统日志

1.为（修改，新建，删除，下发，修改，上传，删除，提交，通过，回退，移除、终止）这些操作提供一个接口，然后前端有对应操作的时候调用我的接口，从而产生记录并记录在数据库中 （实现增这种操作）
2.查询接口，根据条件查询

- 台账
应该也是实现对于数据库信息的查询

路径：package com.mixtower.controller;

```java
/**
 * <p>
 * 系统日志 前端控制器
 * </p>
 *
 * @author wei
 * @since 2022-03-23
 */
@RestController
@RequestMapping("/h-log")
@Api(tags = "系统日志")
public class HLogController {

    @Resource
    private HLogService hLogService;

    @GetMapping("test")
    @ApiOperation(value = "测试日志")
    public  Page<HLog>  test(){

        return hLogService.test();
    }
}

```

路径：package com.mixtower.service.impl;
```java
/**
 * <p>
 * 系统日志 服务实现类
 * </p>
 *
 * @author wei
 * @since 2022-03-23
 */
@Service
public class HLogServiceImpl extends ServiceImpl<HLogMapper, HLog> implements HLogService {

    @Override
    public Page<HLog> test() {
        return this.baseMapper.selectPage(new Page<>(1, 10), null);
    }
}

```
  
路径：package com.mixtower.service;
```java
/**
 * <p>
 * 系统日志 服务类
 * </p>
 *
 * @author wei
 * @since 2022-03-23
 */
public interface HLogService extends IService<HLog> {
    Page<HLog> test();
}

```

```java
@GetMapping("addlog")
    @ApiOperation(value = "新增日志")
    public void add(HLog hlog){

         hLogService.saveLog(hlog);
    }
    
```
```java
 @Async
    @Override
    public void saveLog(HLog hlog){
        //插入HLog操作
        baseMapper.insert(hlog);
    }
```

# Controller的相关注释
- @Api：用在请求的类上，表示对类的说明
    tags="说明该类的作用，可以在UI界面上看到的注解"
    value="该参数没什么意义，在UI界面上也看到，所以不需要配置"
 
 
- @ApiOperation：用在请求的方法上，说明方法的用途、作用
    value="说明方法的用途、作用"
    notes="方法的备注说明"
 **加粗样式**
 
- @ApiImplicitParams：用在请求的方法上，表示一组参数说明
    @ApiImplicitParam：用在@ApiImplicitParams注解中，指定一个请求参数的各个方面
        name：参数名
        value：参数的汉字说明、解释
        required：参数是否必须传
        paramType：参数放在哪个地方
            · header --> 请求参数的获取：@RequestHeader
            · query --> 请求参数的获取：@RequestParam
            · path（用于restful接口）--> 请求参数的获取：@PathVariable
            · body（不常用）
            · form（不常用）    
        dataType：参数类型，默认String，其它值dataType="Integer"       
        defaultValue：参数的默认值
 
 
- @ApiResponses：用在请求的方法上，表示一组响应
    @ApiResponse：用在@ApiResponses中，一般用于表达一个错误的响应信息
        code：数字，例如400
        message：信息，例如"请求参数没填好"
        response：抛出异常的类
 
 
- @ApiModel：用于响应类上，表示一个返回响应数据的信息
            （这种一般用在post创建的时候，使用@RequestBody这样的场景，
            请求参数无法使用@ApiImplicitParam注解进行描述的时候）
    @ApiModelProperty：用在属性上，描述响应类的属性@Api：用在请求的类上，表示对类的说明
    tags="说明该类的作用，可以在UI界面上看到的注解"
    value="该参数没什么意义，在UI界面上也看到，所以不需要配置"
 
 
- @ApiOperation：用在请求的方法上，说明方法的用途、作用
    value="说明方法的用途、作用"
    notes="方法的备注说明"
 
 
- @ApiImplicitParams：用在请求的方法上，表示一组参数说明
    @ApiImplicitParam：用在@ApiImplicitParams注解中，指定一个请求参数的各个方面
        name：参数名
        value：参数的汉字说明、解释
        required：参数是否必须传
        paramType：参数放在哪个地方
            · header --> 请求参数的获取：@RequestHeader
            · query --> 请求参数的获取：@RequestParam
            · path（用于restful接口）--> 请求参数的获取：@PathVariable
            · body（不常用）
            · form（不常用）    
        dataType：参数类型，默认String，其它值dataType="Integer"       
        defaultValue：参数的默认值
 
 
- @ApiResponses：用在请求的方法上，表示一组响应
    @ApiResponse：用在@ApiResponses中，一般用于表达一个错误的响应信息
        code：数字，例如400
        message：信息，例如"请求参数没填好"
        response：抛出异常的类
 
原文：[链接](https://blog.csdn.net/justry_deng/article/details/80972817) 
- @ApiModel：用于响应类上，表示一个返回响应数据的信息
            （这种一般用在post创建的时候，使用@RequestBody这样的场景，
            请求参数无法使用@ApiImplicitParam注解进行描述的时候）
    @ApiModelProperty：用在属性上，描述响应类的属性

    @RequestBody主要用来接收前端传递给后端的json字符串中的数据的(请求体中的数据的)；而最常用的使用请求体传参的无疑是POST请求了，所以使用@RequestBody接收数据时，一般都用POST方式进行提交。在后端的同一个接收方法里，@RequestBody与@RequestParam()可以同时使用，@RequestBody最多只能有一个，而@RequestParam()可以有多个。

- 添加日志操作

即当用户做出某种符合要求的操作时候，在数据库中添加一行操作日志
- entity层 实体类
```java
/**
 * <p>
 * 系统日志
 * </p>
 *
 * @author wei
 * @since 2022-03-23
 */
@Data
@EqualsAndHashCode(callSuper = false)
@Accessors(chain = true)
@TableName("h_log")
@ApiModel(value="HLog对象", description="系统日志")
public class HLog implements Serializable {

    private static final long serialVersionUID = 1L;

    @TableId(value = "id", type = IdType.AUTO)
    private Integer id;

    @ApiModelProperty(value = "账号")
    @TableField("username")
    private String username;

    @ApiModelProperty(value = "姓名")
    @TableField("userfullname")
    private String userfullname;

    @ApiModelProperty(value = "操作时间")
    @TableField("operation_time")
    private LocalDateTime operationTime;

    @ApiModelProperty(value = "菜单模块")
    @TableField("module")
    private String module;

    @ApiModelProperty(value = "操作详情")
    @TableField("details")
    private String details;

    @ApiModelProperty(value = "服务器ip")
    @TableField("server_ip")
    private String serverIp;

    @ApiModelProperty(value = "访问者ip")
    @TableField("visitor_ip")
    private String visitorIp;

    @ApiModelProperty(value = "创建时间")
    @TableField("create_time")
    private LocalDateTime createTime;

    @ApiModelProperty(value = "数据状态")
    @TableField("status")
    private Boolean status;

    public static final String ID = "id";

    public static final String USERNAME = "username";

    public static final String USERFULLNAME = "userfullname";

    public static final String OPERATION_TIME = "operation_time";

    public static final String MODULE = "module";

    public static final String DETAILS = "details";

    public static final String SERVER_IP = "server_ip";

    public static final String VISITOR_IP = "visitor_ip";

    public static final String CREATE_TIME = "create_time";

    public static final String STATUS = "status";

}
```
- vo层（VO则主要用于逻辑层和表示层之间数据处理封装）
```java
/**
 * 系统日志.
 *
 * @author wei
 * @since 2022-03-25
 */
@Data
@EqualsAndHashCode(callSuper = false)
@Accessors(chain = true)
@ApiModel(value="HLogRequestVO", description="系统日志")
public class HLogRequestVO implements Serializable {

    private static final long serialVersionUID = 1L;

    private Integer id;

    @ApiModelProperty(value = "账号")
    private String username;

    @ApiModelProperty(value = "姓名")
    private String userfullname;

    @ApiModelProperty(value = "操作时间")
    private LocalDateTime operationTime;

    @ApiModelProperty(value = "菜单模块")
    private String module;

    @ApiModelProperty(value = "操作详情")
    private String details;

    @ApiModelProperty(value = "服务器ip")
    private String serverIp;

    @ApiModelProperty(value = "访问者ip")
    private String visitorIp;

    @ApiModelProperty(value = "创建时间")
    private LocalDateTime createTime;

    @ApiModelProperty(value = "数据状态")
    private Boolean status;

    @ApiModelProperty(value = "对象拷贝映射器")
    public static Mapper MAPPER = Mappers.getMapper(Mapper.class);

    @org.mapstruct.Mapper
    public interface Mapper {
        @Mappings({
        })
        HLog convert(HLogRequestVO vo);

        @Mappings({
        })
        void update(@MappingTarget HLog dto, HLogRequestVO vo);
    }
}

```

controller层
```java
/**
     * @author shichenbin
     * @describe 每进行产生日志的相关操作时候调用这个接口，该接口在数据库中添加一条日志信息
     * @ApiImplicitParams：用在请求的方法上，表示一组参数说明,参数名
     * */
    @PostMapping("addLog")
    @ApiOperation(value = "添加日志", httpMethod = "POST")
    @ApiImplicitParam(paramType = "body", name = "vo", value = "日志", dataTypeClass = HLog.class, required = true)
    public ResponseModel<Page<HLog>> insertHLog(@RequestBody HLog vo) {

        insertHlog.insertHLog(vo.setCreateTime(LocalDateTime.now()).setStatus(true));

        return null;
    }
```

- dao层（继承mybatis-plus现成的方法）
```java
package com.mixtower.dao;

import com.mixtower.entity.HLog;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;

/**
 * <p>
 * 系统日志 Mapper 接口
 * </p>
 *
 * @author wei
 * @since 2022-03-23
 */
public interface HLogMapper extends BaseMapper<HLog> {

}

```
impl 层
```java
/**
 * <p>
 * 系统日志 服务实现类
 * </p>
 *
 * @author wei
 * @since 2022-03-23
 */

@Service
public class HLogServiceImpl extends ServiceImpl<HLogMapper, HLog> implements HLogService {

    @Override
    public Page<HLog> inquireLog() {

        return this.baseMapper.selectPage(new Page<>(1, 14), null);
    }

    /**
     * @author shichenbin
     * @descirbe 保存操作日志
     */

    @Async
    @Override
    public void saveLog(HLog hlog){
        //插入HLog操作
        baseMapper.insert(hlog);
    }

    @Override
    public int insertHLog(HLog hlog){
        return this.baseMapper.insert(hlog);
    }
}

```


```java
/**
 * <p>
 * 系统日志 服务类
 * </p>
 *
 * @author wei
 * @since 2022-03-23
 */
public interface HLogService extends IService<HLog> {

    Page<HLog> inquireLog();

    //记录日志操作
    void saveLog(HLog hlog);

    int insertHLog(HLog insertHlog);
}

```
