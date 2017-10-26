# API测试步骤

### 测试环境的搭建
* PHPStorm安装
* xdebug扩展安装
* phpunit安装
* phing安装
* jenkins(自动化持续集成工具)安装

### Curl脚本编写(utils.php)
post

    `
    function do_Post($url, $fields, $extraheader = array()){
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL, $url);
        curl_setopt($ch, CURLOPT_POST, true);
        curl_setopt($ch, CURLOPT_POSTFIELDS, $fields );
        curl_setopt($ch, CURLOPT_HTTPHEADER, $extraheader);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true); // 获取数据返回
        $output = curl_exec($ch);
        curl_close($ch);
        return $output;
    }
    
    `
    
get

    `
    function do_Get($url, $extraheader = array()){
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL, $url);
        curl_setopt($ch, CURLOPT_HTTPHEADER, $extraheader);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true); // 获取数据返回:
        //curl_setopt($ch, CURLOPT_VERBOSE, true);
        $output = curl_exec($ch) ;
        curl_close($ch);
        return $output;
    }
    `
    
### restful.php

    `
    require_once 'utils.php';
    
    define("PREFIX", "http://banma.app");
    
    function build_get_param($param) {
        return http_build_query($param);
    }
    
    `
    
### DemoTest.php

    `
    require_once 'restful.php';
    
    use PHPUnit\Framework\TestCase;
    
    define("RESOURCE_ID", 12);
    define("VENDOR_ID", 6);
    
    class ResourceControllerTest extends TestCase
    {
        /**
         * 测试获取多供应商信息接口
         * @url index.php?m=admin&c=resource&a=get_resource_mul_vender_info
         */
        public function testGetResourceMultiVendorInfo()
        {
            $param = [
                'm' => 'admin',
                'c' => 'resource',
                'a' => 'get_resource_mul_vender_info',
                'resourceId' => RESOURCE_ID,
            ];
            $url = "/index.php";
    
            return $this->call_http($url, $param, 'GET');
        }
    
        /**
         * 测试设置优选供应商
         * @url index.php?m=admin&c=resource&a=first_vender_set
         */
        public function testSetFirstVendorSet()
        {
            $param = [
                'm' => 'admin',
                'c' => 'resource',
                'a' => 'first_vender_set',
                'resourceId' => RESOURCE_ID,
                'venderId' => VENDOR_ID,
            ];
            $url = "/index.php";
    
            return $this->call_http($url, $param, 'GET');
    
        }
    
        private function call_http($path, $param, $method, $expect = 0) {
            $_param = build_get_param($param);
            $url = PREFIX . "$path?" . $_param;
            $buf = do_Get($url);
    //        $buf = strcasecmp($method, 'GET') ? do_Get($url) : '';
            $obj = json_decode($buf, True);
            $this->assertEquals($obj['code'], $expect);
            return $obj;
        }
    }
    
    
    `
