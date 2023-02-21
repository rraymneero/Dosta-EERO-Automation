# Dosta-EERO-Reboot Stress Automation

[Jira] CONN-25675- https://eeroinc.atlassian.net/browse/CONN-25675

[Problem] Automate Wi-Fi-Stress EERO network reboot and check for WiFi reconnection (100 iterations)

[Solution] Added stress network reboot code.

[Test] Tested locally in pycharm.

[tested case link]

https://testrail.eero.amazon.dev/index.php?/tests/view/172468

https://testrail.eero.amazon.dev/index.php?/tests/view/172469

https://testrail.eero.amazon.dev/index.php?/tests/view/172467

Stress reboot Code:

[Stress reboot code.txt](https://github.com/rraymneero/Dosta-EERO-Automation/files/10792088/Stress.reboot.code.txt)

      import time

      from selenium import webdriver

      from selenium.webdriver.common.keys import Keys

      from selenium.webdriver.firefox.service import Service

      from webdriver_manager.firefox import GeckoDriverManager

      from selenium.webdriver.common.by import By
 
      driver = webdriver.Firefox(service=Service(executable_path=GeckoDriverManager().install()))

      driver.get("https://admin.stage.e2ro.com/networks/402079")

      driver.find_element(By.XPATH, '/html/body/div/div/div[2]/div/div/div/button').click()

      time.sleep(60)

      stress_itr = 5

      b = 0

      for i in range(0, stress_itr):

      print("iterations:", i)

      driver.find_element(By.XPATH, "//button[@class='ant-btn ant-btn-primary ant-btn-sm']").click()

          time.sleep(10)

          driver.find_element(By.XPATH, "//button[text()='Reboot']").click()

          print(driver.title)

          time.sleep(170)

          driver.find_element(By.XPATH, "//button[@class='ant-btn ant-btn-primary ant-btn-sm']").click()

          time.sleep(10)

          if driver.find_element(By.XPATH, "//*[@class='label label-default']").text == "0":

              print("False, Iteration has failed, proceeding to next Iteration")

          else:

              driver.find_element(By.XPATH, "//button[@class='ant-btn ant-btn-primary ant-btn-sm']").click()

              driver.find_element(By.XPATH, "//a[@class='collapsed' and text()='Connected Devices']").click()

              time.sleep(10)

              text = driver.find_element(By.XPATH, "//div[@class='eo-table eo-table-striped ']").text

              print(text)

              time.sleep(10)

              driver.find_element(By.XPATH, "//a[@class='' and text()='Connected Devices']").click()

              time.sleep(60)

              driver.find_element(By.XPATH, "//button[@class='ant-btn ant-btn-primary ant-btn-sm']").click()

              b = b+1

      print(b, "out of 5")
