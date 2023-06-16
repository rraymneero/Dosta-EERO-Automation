CONN-28863 - EERO automation - Bridge mode

[Problem] Automate - Bridge mode

[Solution] Added Appium Java code which automates Dogfood APP.

[Test] Tested locally in Eclipse.

[tested case link]
https://testrail.eero.amazon.dev/index.php?/cases/view/2694

Instruction to use the script:

[EERO-Automation Bridge Mode_62ff78cc71c74d29b789161f1f489404-160623-0448-2980.pdf](https://github.com/rraymneero/Dosta-EERO-Automation/files/11766089/EERO-Automation.Bridge.Mode_62ff78cc71c74d29b789161f1f489404-160623-0448-2980.pdf)


Note: Attached video logs in jira [CONN-28828](https://eeroinc.atlassian.net/browse/CONN-28828)

Java Code:
[https://drive.corp.amazon.com/documents/rraymn@/EERO%20Automatio/Advance%20Setting/C2688.java](https://drive.corp.amazon.com/personal/rraymn/EERO%20Automatio/Bridge%20Mode)

Code:
               package appium;
        
        import org.testng.annotations.AfterMethod;
        import org.testng.annotations.Test;
        import org.testng.annotations.BeforeMethod;
        import io.appium.java_client.AppiumBy;
        import io.appium.java_client.MobileElement;
        import io.appium.java_client.TouchAction;
        import io.appium.java_client.android.Activity;
        import io.appium.java_client.android.AndroidDriver;
        import io.appium.java_client.android.nativekey.AndroidKey;
        import io.appium.java_client.android.nativekey.KeyEvent;
        import io.appium.java_client.remote.MobileCapabilityType;
        import io.appium.java_client.touch.TapOptions;
        import io.appium.java_client.touch.offset.PointOption;
        import io.appium.java_client.android.AndroidTouchAction;
        
        import org.apache.commons.io.FileUtils;
        import org.openqa.selenium.By;
        import org.openqa.selenium.Keys;
        import org.openqa.selenium.OutputType;
        import org.openqa.selenium.TakesScreenshot;
        import org.openqa.selenium.WebElement;
        import org.openqa.selenium.remote.DesiredCapabilities;
        import org.openqa.selenium.interactions.touch.TouchActions;
        
        import java.io.File;
        import java.io.IOException;
        import java.net.MalformedURLException;
        import java.net.URL;
        
        public class C2694 {
        
        	private AndroidDriver driver;
            
        	 // App1 capabilities
        	    String eeroAppPackageName="com.eero.android.dogfood";
        	    String eeroAppActivityName="com.eero.android.v3.features.splash.SplashActivity";
        
        	   // App2 capabilities
        	    String wifimanAppPackageName="com.ubnt.usurvey";
        	    String wifimanAppActivityName="com.ubnt.usurvey.ui.splash.SplashActivity";
        	    
        	    // App3 capabilities
        	    String PingToolAppPackageName="ua.com.streamsoft.pingtools";
        	    String PingToolAppActivityName="ua.com.streamsoft.pingtools.MainActivity_AA";
        
        		// Bridge Mode
        	    @BeforeMethod
        		public void setUp() throws MalformedURLException {
        	        DesiredCapabilities desiredCapabilities = new DesiredCapabilities();
        	        desiredCapabilities.setCapability("appium:automationName", "UiAutomator2");
        	        desiredCapabilities.setCapability("platformName", "Android");
        	        desiredCapabilities.setCapability("appium:udid", "db4939c");
        	        desiredCapabilities.setCapability("appium:appPackage", "com.eero.android.dogfood");
        	        desiredCapabilities.setCapability("appium:appActivity", "com.eero.android.v3.features.splash.SplashActivity");
        	        desiredCapabilities.setCapability("appium:noReset", true);
        	        desiredCapabilities.setCapability(MobileCapabilityType.NEW_COMMAND_TIMEOUT, 300);
        
        	        URL remoteUrl = new URL("http://127.0.0.1:4723");
        
        	        driver = new AndroidDriver(remoteUrl, desiredCapabilities);
        	    }
        
        	    
        		@Test
        	    public void sampleTest() throws InterruptedException, IOException {
        	        driver.findElement(By.id("com.eero.android.dogfood:id/home")).click();
        	        Thread.sleep(3000); // Sleep for 1000 milliseconds (1 second)
        	        
        	   
        	      // Enter settings
        	        driver.findElement(By.id("com.eero.android.dogfood:id/settings")).click();
        	        Thread.sleep(3000); // Sleep for 1000 milliseconds (1 second)
        	      // Enter Network settings
        	        driver.findElement(By.id("com.eero.android.dogfood:id/advanced")).click();
        	        Thread.sleep(3000);
        	      //Enter DHCP&Nat
        	        driver.findElement(By.xpath("(//android.view.ViewGroup)[11]")).click();
        	        Thread.sleep(3000);
        	      // Change from Automatic to Bridge
        	        driver.findElement(By.xpath("(//android.view.ViewGroup)[5]")).click();
        	        Thread.sleep(3000);
        	      //Save the change
        	        driver.findElement(By.id("com.eero.android.dogfood:id/save_button")).click();
        	        Thread.sleep(3000);
        	      //Reboot to save the change
        	        driver.findElement(By.id("android:id/button1")).click();
        	        Thread.sleep(180000);
        	        
        	        //launch ping app to check ping
        	        DesiredCapabilities capabilities = new DesiredCapabilities(driver.getCapabilities().asMap());
        	        capabilities.setCapability("appium:appPackage", PingToolAppPackageName);
        	        capabilities.setCapability("appium:appActivity", PingToolAppActivityName);
        	        URL remoteUrl2 = new URL("http://127.0.0.1:4723");
        	        driver = new AndroidDriver(remoteUrl2, capabilities);
        	        capabilities.setCapability("appium:noReset", true);
        	        
        	        Thread.sleep(10000);
        	        
        	        // take screenshot
        	        File scrFile1 = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
        
        	        // specify the destination file path
        	        String filePath1 = "C:\\Users\\wiz\\eclipse-workspace\\appium\\C2694_Bridge mode\\BridgeModeIpAddress.png";
        	        FileUtils.copyFile(scrFile1, new File(filePath1));
        
        	        System.out.println(System.getProperty("user.dir"));
        	        
        	        driver.findElement(By.className("android.widget.ImageButton")).click();
        	        Thread.sleep(5000);
        	        driver.findElement(By.xpath("(//android.widget.CheckedTextView)[4]")).click();
        	        Thread.sleep(5000);
        	        driver.findElement(By.className("android.widget.ToggleButton")).click();
        	        Thread.sleep(10000);
        	        
        	        
        	     // take screenshot
        	        File scrFile2 = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
        
        	        // specify the destination file path
        	        String filePath2 = "C:\\\\Users\\\\wiz\\\\eclipse-workspace\\\\appium\\\\C2694_Bridge mode\\\\BridgeModePing.png";
        	        FileUtils.copyFile(scrFile2, new File(filePath2));
        
        	        System.out.println(System.getProperty("user.dir"));      
        	        Thread.sleep(10000);
        	      //Close ping app       
        	        
        	        driver.terminateApp(PingToolAppPackageName);
        	        
        	     
        	    }
        }
