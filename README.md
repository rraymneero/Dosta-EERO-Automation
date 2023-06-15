[CONN-28828](https://eeroinc.atlassian.net/browse/CONN-28828) - EERO automation - IPv4 Reservations for client device/DHCP remove static reservation 

[Problem] Automate  IPv4 Reservations for client device/DHCP remove static reservation 

[Solution] Added Appium Java code which automates Dogfood APP.

[Test] Tested locally in Eclipse.

[tested case link]
https://testrail.eero.amazon.dev/index.php?/cases/view/2688

Instruction to use the script:

[EERO Advance setting automation_052b712d2c214262ad74792eb4a78bad-150623-0750-2924.pdf](https://github.com/rraymneero/Dosta-EERO-Automation/files/11757765/EERO.Advance.setting.automation_052b712d2c214262ad74792eb4a78bad-150623-0750-2924.pdf)

Note: Attached video logs in jira [CONN-28828](https://eeroinc.atlassian.net/browse/CONN-28828)

Stress reboot Code:
https://drive.corp.amazon.com/documents/rraymn@/EERO%20Automatio/Advance%20Setting/C2688.java

Code:
        package appium;
      
      import io.appium.java_client.AppiumBy;
      import io.appium.java_client.MobileElement;
      import io.appium.java_client.android.Activity;
      import io.appium.java_client.android.AndroidDriver;
      import junit.framework.TestCase;
      
      import org.apache.commons.io.FileUtils;
      import org.junit.After;
      import org.junit.Before;
      import org.junit.Test;
      import org.openqa.selenium.By;
      import org.openqa.selenium.OutputType;
      import org.openqa.selenium.TakesScreenshot;
      import org.openqa.selenium.remote.DesiredCapabilities;
      
      import java.io.File;
      import java.io.IOException;
      import java.net.MalformedURLException;
      import java.net.URL;
      
      public class C2688 {
      
          private AndroidDriver driver;
          
       // App1 capabilities
          String eeroAppPackageName="com.eero.android.dogfood";
          String eeroAppActivityName="com.eero.android.v3.features.splash.SplashActivity";
      
         // App2 capabilities
          String wifimanAppPackageName="com.ubnt.usurvey";
          String wifimanAppActivityName="com.ubnt.usurvey.ui.splash.SplashActivity";
      
      
          @Before
          public void setUp() throws MalformedURLException {
              DesiredCapabilities desiredCapabilities = new DesiredCapabilities();
              desiredCapabilities.setCapability("appium:automationName", "UiAutomator2");
              desiredCapabilities.setCapability("platformName", "Android");
              desiredCapabilities.setCapability("appium:udid", "db4939c");
              desiredCapabilities.setCapability("appium:appPackage", "com.eero.android.dogfood");
              desiredCapabilities.setCapability("appium:appActivity", "com.eero.android.v3.features.splash.SplashActivity");
              desiredCapabilities.setCapability("appium:noReset", true);
      
              URL remoteUrl = new URL("http://127.0.0.1:4723");
      
              driver = new AndroidDriver(remoteUrl, desiredCapabilities);
          }
      
          @Test
          public void sampleTest() throws InterruptedException, IOException {
              driver.findElement(By.id("com.eero.android.dogfood:id/settings")).click();
              Thread.sleep(3000); // Sleep for 1000 milliseconds (1 second)
              
              driver.findElement(By.id("com.eero.android.dogfood:id/advanced")).click();
              Thread.sleep(3000);
              
              driver.findElement(By.xpath("(//android.view.ViewGroup)[13]")).click();
              Thread.sleep(3000);
              
              driver.findElement(By.xpath("(//android.view.ViewGroup)[3]")).click();
              Thread.sleep(3000);
              
              //select device.
              driver.findElement(By.xpath("(//android.view.ViewGroup)[3]")).click();
              
              //Edit ip address
              driver.findElement(By.xpath("(//android.widget.LinearLayout)[7]")).click();
              driver.findElement(By.id("com.eero.android.dogfood:id/inputIpAddress")).sendKeys("192.168.4.55");
              Thread.sleep(3000);
              
              //save the changed ip
              driver.findElement(By.id("com.eero.android.dogfood:id/save")).click();
              Thread.sleep(3000);
              //goes to setting
              driver.findElement(By.id("com.eero.android.dogfood:id/settings")).click();
              Thread.sleep(3000); // Sleep for 1000 milliseconds (1 second)
              //goes to network setting
              driver.findElement(By.id("com.eero.android.dogfood:id/advanced")).click();
              Thread.sleep(3000);
              //reboot network
              driver.findElement(AppiumBy.androidUIAutomator("new UiScrollable(new UiSelector()).scrollIntoView(text(\"Restart network\"))"));
              driver.findElement(By.xpath("(//android.view.ViewGroup)[21]")).click();
              Thread .sleep(30000);
              driver.findElement(By.id("com.eero.android.dogfood:id/restart_eero_button")).click();
              Thread.sleep(220000);
              //go to homepage.
              //driver.findElement(By.id("com.eero.android.dogfood:id/home")).click();
              
              //launch wifiman to check change in ip address.
              DesiredCapabilities capabilities = new DesiredCapabilities(driver.getCapabilities().asMap());
              capabilities.setCapability("appium:appPackage", wifimanAppPackageName);
              capabilities.setCapability("appium:appActivity", wifimanAppActivityName);
              URL remoteUrl = new URL("http://127.0.0.1:4723");
              driver = new AndroidDriver(remoteUrl, capabilities);
              capabilities.setCapability("appium:noReset", true);
              
             Thread.sleep(50000);
              
          // take screenshot
             File scrFile = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
      
             // specify the destination file path
             String filePath = "C:\\Users\\wiz\\eclipse-workspace\\appium\\C2688_IPv4 Reservations for client deviceDHCP remove static reservation\\reservedIP.png";
             FileUtils.copyFile(scrFile, new File(filePath));
      
             System.out.println(System.getProperty("user.dir"));
              
              //Close wifiman app
              
              Thread.sleep(50000);
              
              driver.terminateApp(wifimanAppPackageName);
              
              // open eero app to delete the reserve ip.
              driver.findElement(By.id("com.eero.android.dogfood:id/settings")).click();
              Thread.sleep(3000); // Sleep for 1000 milliseconds (1 second)
              //enter reservation.
              
              driver.findElement(By.id("com.eero.android.dogfood:id/advanced")).click();
              Thread.sleep(3000);
              
              driver.findElement(By.xpath("(//android.view.ViewGroup)[13]")).click();
              Thread.sleep(3000);
              //click saved device.
              
              driver.findElement(By.id("com.eero.android.dogfood:id/label")).click();
              Thread.sleep(3000);
              // click delete reservation.
              driver.findElement(By.id("com.eero.android.dogfood:id/delete_rule_button")).click();
              Thread.sleep(3000);
              
            //launch wifiman to check change in ip address.
              DesiredCapabilities capabilities1 = new DesiredCapabilities(driver.getCapabilities().asMap());
              capabilities1.setCapability("appium:appPackage", wifimanAppPackageName);
              capabilities1.setCapability("appium:appActivity", wifimanAppActivityName);
              URL remoteUrl1 = new URL("http://127.0.0.1:4723");
              driver = new AndroidDriver(remoteUrl1, capabilities1);
              capabilities1.setCapability("appium:noReset", true);
              
             Thread.sleep(50000);
              
          // take screenshot after deleting the reserve ip.
             File scrFile1 = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
      
             // specify the destination file path
             String filePath1 = "C:\\Users\\wiz\\eclipse-workspace\\appium\\C2688_IPv4 Reservations for client deviceDHCP remove static reservation\\DeletedreservedIP.png";
             FileUtils.copyFile(scrFile1, new File(filePath1));
      
             System.out.println(System.getProperty("user.dir"));
              
              //Close wifiman app
              
              Thread.sleep(50000);
              
              driver.terminateApp(wifimanAppPackageName);
              
          
              
          }
      
          @After
          public void tearDown() {
              driver.quit();
          }
      }
