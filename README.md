from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from webdriver_manager.chrome import ChromeDriverManager
import time

# ---------- SETUP ----------
options = webdriver.ChromeOptions()
options.add_argument("--start-maximized")
# options.add_argument("--headless")  # agar background me run chahiye

driver = webdriver.Chrome(
    service=Service(ChromeDriverManager().install()),
    options=options
)

wait = WebDriverWait(driver, 20)

# ---------- 1. OPEN AMAZON ----------
driver.get("https://www.amazon.in")

# ---------- 2. LOGIN ----------
# Sign in button
signin_btn = wait.until(EC.element_to_be_clickable((By.ID, "nav-link-accountList")))
signin_btn.click()

# Email / Mobile
email_input = wait.until(EC.presence_of_element_located((By.ID, "ap_email")))
email_input.send_keys("YOUR_EMAIL_OR_MOBILE")
driver.find_element(By.ID, "continue").click()

# Password
password_input = wait.until(EC.presence_of_element_located((By.ID, "ap_password")))
password_input.send_keys("YOUR_PASSWORD")
driver.find_element(By.ID, "signInSubmit").click()

time.sleep(5)  # captcha aa sakta hai

# ---------- 3. SEARCH IPHONE ----------
search_box = wait.until(EC.presence_of_element_located((By.ID, "twotabsearchtextbox")))
search_box.send_keys("iphone")
search_box.send_keys(Keys.ENTER)

# ---------- 4. SCROLL DOWN ----------
for i in range(3):
    driver.execute_script("window.scrollBy(0, 1000);")
    time.sleep(2)

# ---------- 5. CLICK FIRST PRODUCT ----------
first_product = wait.until(
    EC.element_to_be_clickable((By.CSS_SELECTOR, "div[data-component-type='s-search-result'] h2 a"))
)
first_product.click()

# Switch to new tab
driver.switch_to.window(driver.window_handles[1])

# ---------- 6. ADD TO CART ----------
add_to_cart_btn = wait.until(
    EC.element_to_be_clickable((By.ID, "add-to-cart-button"))
)
add_to_cart_btn.click()

time.sleep(5)

print(
