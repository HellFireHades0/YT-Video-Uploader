from selenium.webdriver import Keys
import fake_useragent
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
import undetected_chromedriver as uc
import json
import os
import time


def save_cookie(driver):
    with open('cookies.json', 'w') as file_handler:
        json.dump(driver.get_cookies(), file_handler)


def load_cookie(driver):
    with open('cookies.json', 'r') as cookies_file:
        cookies = json.load(cookies_file)
    for cookie in cookies:
        driver.add_cookie(cookie)


def upload(email='',
           password='',
           filepath='',
           title='',
           description='',
           made_for_kids=False,
           video_type='public'):
    logged_in = False
    driver = ''

    if __name__ == '__main__' and os.path.isfile('cookies.json'):
        options = Options()
        driver = uc.Chrome(options=options, driver_executable_path=r'C:\Users\user\.wdm\drivers\chromedriver\win32\100.0.4896.60\chromedriver.exe')

        driver.get('https://youtube.com')
        load_cookie(driver)
        driver.refresh()
        logged_in = True

    if __name__ == '__main__' and not os.path.isfile('cookies.json'):

        login_url = 'https://accounts.google.com/AddSession/identifier?service=accountsettings&continue=https%3A%2F%2F'\
                    'myaccount.google.com%2Fsecurity&ec=GAlAwAE&flowName=GlifWebSignIn&flowEntry=AddSession'
        options = Options()
        options.add_argument(f"user-agent={fake_useragent.UserAgent().random}")
        driver = uc.Chrome(options=options, driver_executable_path=r'C:\Users\user\.wdm\drivers\chromedriver\win32\100.0.4896.60\chromedriver.exe')
        driver.maximize_window()
        driver.get(login_url)
        driver.implicitly_wait(4)
        driver.find_element(By.XPATH, '//*[@id ="identifierId"]').send_keys(email)
        next_btn = driver.find_elements(By.XPATH, '//*[@id ="identifierNext"]')
        driver.implicitly_wait(4)
        next_btn[0].click()
        driver.implicitly_wait(5)
        driver.find_element(By.XPATH, '//*[@id ="password"]/div[1]/div / div[1]/input').send_keys(password)
        driver.find_element(By.XPATH, '//*[@id="c3"]').click()
        driver.find_elements(By.XPATH, '//*[@id ="passwordNext"]')[0].click()
        driver.get('https://youtube.com')
        time.sleep(4)
        save_cookie(driver)
        logged_in = True

    if logged_in:
        if made_for_kids:
            made_for_kids = 0
        else:
            made_for_kids = 1
        options = Options()
        options.add_argument(f"user-agent={fake_useragent.UserAgent().random}")
        options.add_argument('--headless')
        options.add_argument('--disable-gpu')

        driver.get('https://youtube.com/upload')

        driver.implicitly_wait(10)
        driver.find_element(By.XPATH, "//input[@type='file']").send_keys(filepath)
        driver.implicitly_wait(10)
        title_of_video = driver.find_element(By.ID, 'textbox')
        title_of_video.click()
        title_of_video.send_keys(Keys.CONTROL + 'a')
        title_of_video.send_keys(title)
        description_of_video = driver.find_elements(By.ID, 'textbox')[1]
        description_of_video.click()
        description_of_video.send_keys(description)
        driver.find_elements(By.ID, 'radioLabel')[made_for_kids].click()
        driver.implicitly_wait(5)
        driver.find_element(By.ID, 'next-button').click()
        driver.implicitly_wait(10)
        driver.find_element(By.ID, 'next-button').click()
        driver.implicitly_wait(10)
        driver.find_element(By.ID, 'next-button').click()
        publish_xpath = ''
        if video_type.lower() == 'public':
            publish_xpath = '//*[@id="privacy-radios"]/tp-yt-paper-radio-button[3]'
        if video_type.lower() == 'unlisted':
            publish_xpath = '//*[@id="privacy-radios"]/tp-yt-paper-radio-button[2]'
        if video_type.lower() == 'private':
            publish_xpath = '//*[@id="private-radio-button"]'

        driver.find_element(By.XPATH, publish_xpath).click()
        driver.implicitly_wait(100)
        for i in driver.find_elements(By.XPATH, '//*[@id="scrollable-content"]/'
                                                'ytcp-uploads-review/div[3]/ytcp-video-info/div/div[3]/div[1]/div[2]'):
            while not str(i.text).startswith('https:'):
                pass
            print(f'Video Url: {i.text}')
        while True:
            progress = driver.find_element(By.XPATH, '/html/body/ytcp-uploads-dialog/tp-yt-paper-dialog/div/ytcp'
                                                     '-animatable[2]/div/div[1]/ytcp-video-upload-progress/span')
            print(progress.text)
            if str(progress.text) == 'Checks complete. No issues found.' or str(progress.text) == 'Checks complete. ' \
                                                                                                  'Copyright claim ' \
                                                                                                  'found.':
                break
            time.sleep(3)

        driver.find_element(By.XPATH, '//*[@id="done-button"]/div').click()
        driver.implicitly_wait(10)
        driver.find_element(By.XPATH, '//*[@id="close-button"]/div').click()
        driver.implicitly_wait(10)
        print('Uploaded')
