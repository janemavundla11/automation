from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import StaleElementReferenceException
from selenium.common.exceptions import TimeoutException, NoSuchElementException
import time

chrome_driver_path = "chromedriver.exe"

service = Service(chrome_driver_path)
driver = webdriver.Chrome(service=service)
driver.implicitly_wait(10)  # Implicit wait
wait = WebDriverWait(driver, 30) 

try:
    driver.get("https://bookuat.michanic.co.za/")

    image_element = driver.find_element(By.CSS_SELECTOR, 'img[alt="lcoation1"]')
    image_element.click()

    audi_element = driver.find_element(By.XPATH, "//img[contains(@src, 'assets/makes/Audi.png')]/following-sibling::h6/span[text()=' Audi ']")
    audi_element.click()
    print("Audi element found and clicked.")

    year_element = driver.find_element(By.XPATH, "//h6[contains(text(), '2019')]")
    year_element.click()
    print("Year 2021 element found and clicked.")

    model_element = driver.find_element(By.XPATH, '//div[@class="car-name-tag car-name-size ng-star-inserted"]//h6[contains(text(), "A3")]')
    model_element.click()

    trim_element = driver.find_element(By.XPATH, '//div[@class="car-name-tag car-name-size ng-star-inserted"]//h6[contains(text(), "Sportback 35TFSI Black Edition")]')
    trim_element.click()
    
    try:
        email_input = wait.until(EC.element_to_be_clickable((By.ID, "input_2502_41307_40334")))
        email_input.clear()
        email_input.send_keys("janem@michanic.co.za")
    except TimeoutException:
        print("The email input field was not found within the specified time.")
    except NoSuchElementException:
        print("The email input field does not exist on the page.")
    
    time.sleep(20)
finally:
    driver.quit()
