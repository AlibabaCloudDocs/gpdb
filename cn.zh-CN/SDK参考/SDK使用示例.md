# SDK使用示例

云原生数据仓库AnalyticDB PostgreSQL版支持Java、Python开发。

## Java版SDK使用示例

-   前提条件：JDK 1.6.0或更高版本。
-   操作步骤：
    1.  安装云原生数据仓库AnalyticDB PostgreSQL版的Java版SDK。

        如果您使用Maven管理Java项目，可以通过在pom.xml文件中添加Maven依赖安装。

        ```
        <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>fastjson</artifactId>
          <version>1.2.37</version>
        </dependency>
        
        <dependency>
          <groupId>com.aliyun</groupId>
          <artifactId>aliyun-java-sdk-core</artifactId>
          <version>4.4.0</version>
        </dependency>
        
        <dependency>
          <groupId>com.aliyun</groupId>
          <artifactId>aliyun-java-sdk-gpdb</artifactId>
          <version>1.0.7</version>
        </dependency>
        ```

    2.  使用Java版SDK。

        以下代码示例展示了调用Java版SDK的三个主要步骤：

        1.  创建DefaultAcsClient实例并初始化。
        2.  创建API请求并设置参数。
        3.  发起请求并处理应答或异常。
        ```
        import com.alibaba.fastjson.JSON;
        import com.aliyuncs.DefaultAcsClient;
        import com.aliyuncs.IAcsClient;
        import com.aliyuncs.exceptions.ClientException;
        import com.aliyuncs.exceptions.ServerException;
        import com.aliyuncs.gpdb.model.v20160503.DescribeSQLCollectorPolicyRequest;
        import com.aliyuncs.gpdb.model.v20160503.DescribeSQLCollectorPolicyResponse;
        import com.aliyuncs.profile.DefaultProfile;
        
        public class GpdbDemo2 {
        
            public static void main(String args[]) {
                 /*
                 创建DefaultAcsClient实例并初始化
                 ${region}：地区名称
                 ${accessKey}：用户accessKey
                 ${secret}：用户密钥
                 */
                DefaultProfile profile = DefaultProfile.getProfile("${region}",
                        "${accessKey}", "${secret}");
                IAcsClient client = new DefaultAcsClient(profile);
                // 创建API请求并设置参数
                String insName = "${insName}";
                DescribeSQLCollectorPolicyRequest request
        = new DescribeSQLCollectorPolicyRequest();
                request.setDBInstanceId(insName);
                // 发起请求并处理应答或异常
                try {
                    DescribeSQLCollectorPolicyResponse response
        = client.getAcsResponse(request);
                   System.out.println(JSON.toJSONString(response));
                } catch (ServerException e) {
                    e.printStackTrace();
                } catch (ClientException e) {
                   System.out.println("ErrCode:" + e.getErrCode());
                   System.out.println("ErrMsg:" + e.getErrMsg());
                   System.out.println("RequestId:" + e.getRequestId());
                }
        
            }
        }
        ```


## Python版SDK使用示例

-   前提条件：Python 2或Python 3。
-   操作步骤：
    1.  使用 pip 命令，安装云原生数据仓库AnalyticDB PostgreSQL版的SDK示例：

        ```
        pip install aliyun-python-sdk-gpdb
        ```

    2.  使用Python版SDK。

        以下代码示例为Python 2版本，展示了调用阿里云Python SDK的3个主要步骤：

        1.  创建Client实例。在创建Client实例时，您需要获取Region ID、AccessKey ID和AccessKey Secret。
        2.  创建API请求并设置参数。
        3.  发起请求并处理应答或异常。
        ```
        from aliyunsdkcore.client import AcsClient
        from aliyunsdkcore.acs_exception.exceptions import ClientException
        from aliyunsdkcore.acs_exception.exceptions import ServerException
        from aliyunsdkgpdb.request.v20160503.DescribeSQLCollectorPolicyRequest import DescribeSQLCollectorPolicyRequest
        # 创建AcsClient实例 
        # ${accessKey}: 用户accessKey
        # ${accessSecret}：用户密钥
        # ${regionId}：地区编码
        client = AcsClient(
                "${accessKey}",
                "${accessSecret}",
                "${regionId}"
                )
        # 创建request，并设置参数
        request = DescribeSQLCollectorPolicyRequest()
        # ${insName}: 实例名称
        request.set_DBInstanceId("${insName}")               
        # 发起API请求并显示返回值 
        try:
            response = client.do_action_with_exception(request)
            print response
        except ServerException as e:
            print e
        except ClientException as e:
            print e
        ```


