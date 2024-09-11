# Automating-reCAPTCHA-Solving-with-Selenium
<p>As an automation enthusiast, I’ve come across the common challenge of bypassing reCAPTCHA. While there are numerous guides out there, sharing personal experiences can be insightful.</p>
<p>I based my approach on a manual by 2Captcha, a captcha recognition service, and decided to put it to the test.</p>
<p>Preparation: Firstly, ensure your environment is set up correctly. Install Python and then download the necessary components:</p>
<ul>
  <li>Selenium: A library for browser automation. Available at PyPI (https://pypi.org/project/selenium/).</li>
  <li>2Captcha Python SDK: Official Python SDK for integrating with the 2Captcha API. Get it from PyPI (https://pypi.org/project/selenium/).</li>
  <li>Webdriver-manager: Simplifies the management of Selenium drivers. Download it from PyPI (https://pypi.org/project/selenium/).</li>
</ul>
<p>The installation is straightforward: copy and run the provided commands in your terminal. Or, use the universal command:</p>
<per>
  python -m pip install 2captcha-python selenium webdriver-manager
</per>
<p>Finding the Sitekey: The <code>site key</code>site key is a unique identifier for Google’s reCAPTCHA forms. To allow the captcha service to recognize the captcha to be solved, you need this identifier.</p>
<p>Solving the Captcha: Develop a script that navigates to the target page and solves the captcha via an API, providing the solution code.</p>
<p>Submitting the Solved Captcha: The next step involves finding the <code>g-recaptcha-response</code> element, inserting the solution code, and submitting the form.</p>
<p>Here’s a snippet to demonstrate this action:</p>
<per>
  recaptcha_response_element = driver.find_element(By.ID, 'g-recaptcha-response')
driver.execute_script(f'arguments[0].value = "{code}";', recaptcha_response_element)

submit_btn = driver.find_element(By.CSS_SELECTOR, 'button[type="submit"]')
submit_btn.click()
</per>
<p>Complete Code for Automating reCaptcha Resolution:</p>
<p>The complete Python script integrates all steps. It instantiates the WebDriver, loads the target page, solves the captcha, sets the solution in the webpage, and submits it. The full code is as follows:</p>
<per>
  from selenium.webdriver.common.by import By
from twocaptcha import TwoCaptcha
from selenium.webdriver.chrome.service import Service as ChromeService
from webdriver_manager.chrome import ChromeDriverManager
from selenium import webdriver

# Initialize the WebDriver
driver = webdriver.Chrome(service=ChromeService(ChromeDriverManager().install()))

# Go to the target page
captcha_page_url = "https://recaptcha-demo.appspot.com/recaptcha-v2-checkbox.php"
driver.get(captcha_page_url)

# Start solving the Captcha
print("Solving Captcha")
solver = TwoCaptcha("2CAPTCHA_API_KEY")
response = solver.recaptcha(sitekey='SITE_KEY', url=captcha_page_url)
code = response['code']
print(f"Successfully solved the Captcha. The solve code is {code}")

# Insert the solved Captcha response
recaptcha_response_element = driver.find_element(By.ID, 'g-recaptcha-response')
driver.execute_script(f'arguments[0].value = "{code}";', recaptcha_response_element)

# Submit the form
submit_btn = driver.find_element(By.CSS_SELECTOR, 'button[type="submit"]')
submit_btn.click()

# Wait for the user to see the result before closing
input("Press enter to continue")
driver.close()
</per>
<p>Feel free to use it, and don’t forget to thank the author with a like!</p>
