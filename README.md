# webrtc-test
Framework for functional and Load Testing of WebRTC

## Install prerequisites
#### CentOS 7
* Install janus: https://github.com/meetecho/janus-gateway
* Install python utilities: `pip install psutil matplotlib`

#### Ubuntu 16.04
* Install chrome: 
```bash
$ wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
$ sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
$ sudo apt-get update
$ sudo apt-get install google-chrome-stable
```
* Download chromedriver from https://sites.google.com/a/chromium.org/chromedriver/downloads
* Install selenium: `pip install selenium`
* Install pytest:  `pip install pytest pytest-xdist pytest-rerunfailures`

## Initiate load tests ##
#### 1. Start monitoring
* on CentOS
```python
# psmon.sh

import sys
import pmm

import matplotlib
matplotlib.use('Agg')

if __name__ == '__main__':
    sys.exit(pmm.main())
```
`$ psmon.sh <PID> --log <FILENAME> --plot <FILENAME> --interval <SECS> --duration <SECS>`
 
#### 2. Run webtest
* on Ubuntu
```python
# webtest.py

from selenium import webdriver

options = webdriver.ChromeOptions()
options.add_argument('headless')
options.add_argument('window-size=1280x720')
options.add_argument("disable-gpu")
options.add_argument('no-sandbox')
options.add_argument("use-fake-device-for-media-stream");
options.add_argument("use-fake-ui-for-media-stream");

driver = webdriver.Chrome(<CHROMEDRIVER_PATH>, chrome_options=options)
driver.get(<URL>)
driver.implicitly_wait(30)
driver.find_element_by_id("start").click()

#driver.quit()
```
`$ py.test -n <numprocesses> webtest.py`

#### 3. End monitoring
* Terminate process with **<CTRL+C>** on CentOS
* Log files generation and test results recording

## Analyze the results ##
a quad core CPU with 8GB RAM, 640x480 1Mbits/s
![pslog.txt](images/pslog_txt.png)
![pslog.png](images/pslog.png)

<hr/>

### GStreamer (with janus streaming plugin)
* Install gstreamer: `$ yum install gstreamer1*`
* Start gstreamer: `$ /opt/janus/share/janus/streams/test_gstreamer_1.sh`
