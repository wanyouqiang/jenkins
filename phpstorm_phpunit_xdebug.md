# phpstorm+phpunit+xdebug进行单元测试

### 下载phpunit.phar,创建phpunit.bat文件，配置环境变量
* 下载phpunit.phar(https://phpunit.de/)

* 创建phpunit.bat

    `
        //phpunit.bat
        @php "%~dp0phpunit.phar" %*
        
    `
### 修改php.ini开启xdebug扩展

### 在phpstorm中集成phpunit
* 在设置中指定本地phpunit.phar位置

### 编写测试用例

    `
        class DemoTest extends \PHPUnit\Framework\TestCase
        {
            /**
             * 对输出进行测试
             */
            public testIndexAction()
            {
                $this->expectOutputString('foo');
                print 'foo';
            }
            
            /**
             * 对变量值进行测试
             */
             public testVarAction()
             {
                $foo = "phpunit";
                $this->assertEquals("phpunit", $foo);
             }
             
             /**
              * 对错误进行测试
              * @expectedException PHPUnit\Framework\Error
              */
              public function testFailingInclude()
              {
                  include 'not_existing_file.php';
              }
              
             /**
              * 对布尔逻辑进行测试
              */
              public function testBooAction()
              {
                  $this->assertFalse(false);
              }
        }
    `