# heroku-buildpack-chromedriver

This buildpack installs
[`chromedriver`](https://sites.google.com/a/chromium.org/chromedriver/)
 (the Selenium driver for Chrome) in a Heroku slug.
 
 This buildpack only installs the `chromedriver` binary. To use Selenium with Chrome
 on Heroku, you'll also need Chrome. We suggest one of these buildpacks:
 
 - [heroku-buildpack-google-chrome](https://github.com/heroku/heroku-buildpack-google-chrome) 
   to run Chrome with the `--headless` flag
 - [heroku-buildpack-xvfb-google-chrome](https://github.com/heroku/heroku-buildpack-xvfb-google-chrome)
   to run Chrome against a virtual window server


## Configuring the downloaded version of chromedriver.

~~By default, this buildpack will download the latest release, which is provided
by [Google](https://chromedriver.storage.googleapis.com/LATEST_RELEASE).~~

~~You can control the specific version by setting the `CHROMEDRIVER_VERSION`
variable to an explicit version e.g. `2.39`.~~

I modified the compile script to check what the dowloaded google chrome version is, then download a correspond chromedriver.
This should help me get over this error.
```
selenium.common.exceptions.SessionNotCreatedException: Message: session not created: This version of ChromeDriver only supports Chrome version 81
```

## Buildpack
![alt text](https://github.com/kritsana-chaikaew/heroku-buildpack-chromedriver/blob/master/buildpack-order.png "buildpack order")

## Selenium Setup
```python
from selenium import webdriver

GOOGLE_CHROME_PATH = '/app/.apt/usr/bin/google-chrome'
CHROMEDRIVER_PATH = '/app/.chromedriver/bin/chromedriver'

chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument('--disable-gpu')
chrome_options.add_argument('--no-sandbox')
chrome_options.binary_location = GOOGLE_CHROME_PATH
browser = webdriver.Chrome(executable_path=CHROMEDRIVER_PATH, chrome_options=chrome_options)
```

## Releasing a new version

Make sure you publish this buildpack in the buildpack registry

`heroku buildpacks:publish heroku/chromedriver master`
