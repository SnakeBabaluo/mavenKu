# sellingpartner-api-aa-java
## 这是生成的Amazon spapi依赖
使用说明:
1. 声明下载地址
```java
<repositories>
  <repository>
    <id>luodapao</id>
    <name>luodapao</name>
    <url>https://raw.github.com/SnakeBabaluo/sellingpartner-api-aa-java/master/</url>
  </repository>
</repositories>
```
2. 导入maven下载
```java
<dependency>
      <!--要与打包的项目一样-->
	<groupId>com.amazon.sellingpartnerapi</groupId>
	<artifactId>sellingpartnerapi-aa-java</artifactId>
	<version>1.0</version>
</dependency>
```
   
## 用例
```java
import com.amazon.SellingPartnerAPIAA.AWSAuthenticationCredentials;
import com.amazon.SellingPartnerAPIAA.AWSAuthenticationCredentialsProvider;
import com.amazon.SellingPartnerAPIAA.LWAAuthorizationCredentials;
import com.amazon.client.ApiException;
import com.amazon.client.api.OrdersV0Api;
import com.amazon.client.model.GetOrdersResponse;
import org.junit.Ignore;
import org.junit.Test;

import java.util.ArrayList;
import java.util.List;

/**
 * @author LuoDaPao
 * @version 1.0
 * GetOrderTest Created on 2022-10-10 下午 下午 3:47
 */
@Ignore
public class GetOrderTest {

    @Test
    public void amazonAuthorizationGrant() throws ApiException {
        OrdersV0Api ordersV0Api = new OrdersV0Api.Builder()
                .awsAuthenticationCredentials(AWSAuthenticationCredentials.builder()
                        //IAM user的accessKeyId
                        .accessKeyId("")
                        //IAM user的secretKey
                        .secretKey("")
                        //这里按照amazon对不同region的分区填写，例子是北美地区的
                        .region("us-east-1")
                        .build())
                .lwaAuthorizationCredentials(LWAAuthorizationCredentials.builder()
                        //申请app后LWA中的clientId
                        .clientId("")
                        //申请app后LWA中的clientSecret
                        .clientSecret("")
                        //店铺授权时产生的refreshToken或者app自授权生成的
                        .refreshToken("")
                        .endpoint("https://api.amazon.com/auth/o2/token")
                        .build())
                .awsAuthenticationCredentialsProvider(AWSAuthenticationCredentialsProvider.builder()
                        //IAM role，特别注意：最好用IAM role当做IAM ARN去申请app
                        // 而且IAM user需要添加内联策略STS关联上IAM role，具体操作看：https://www.spapi.org.cn/cn/model2/_2_console.html
                        .roleArn("")
                        .roleSessionName("spapi")
                        .build())
                //注意，这里的endpoint分北美，欧洲，远东三个地域，每个区域的链接是不一样的
                //北美，https://sellingpartnerapi-na.amazon.com
                //欧洲，https://sellingpartnerapi-eu.amazon.com
                //远东，https://sellingpartnerapi-fe.amazon.com
                .endpoint("https://sellingpartnerapi-na.amazon.com")
                .build();

        //授权失败，未获取到API实例的话抛出异常，进行重试
        if (null == ordersV0Api) {
            throw new RuntimeException();
        }
        List<String> marketplaceIds = new ArrayList<>();
        marketplaceIds.add("ATVPDKIKX0DER");
        GetOrdersResponse orders = ordersV0Api.getOrders(marketplaceIds, "1999-10-01", null,
                null, null, null, null, null, null, null
                , 2, null, null, null, null, null, null, null);
        System.out.println(orders);
    }
}
```
