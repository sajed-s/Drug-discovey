# Drug-discovey
In this section, I'm going two explain two possible ways to extract the different descriptors of a list of molecule.

- In the first part, you will find a simple code that I wrote in google collab. This code will tell you the Lipinski parameters of your component.
- In the second part, I wrote a python code using a chrome driver to extract more descriptors. This code will import your SMILEs list to ADMET2 and download the result as a CSV file.

## Part 1

All the information can be found in the "Drug-discovery.ipynb" file

## part 2

First, you need to add a chrome driver to your repository
- you can download it from here: https://chromedriver.chromium.org/downloads
- Remember the driver version should be the same as your chrome version. Moreover, you can use other drivers if you use another browser
- There are some tutorials on how to use chrome driver with google collab. I personally do not recommend using this.

## Installation

Install the selenium using this command: 


`1. pip install selenium`

## Python transcript

- First, import the libraies:

```python
import pandas as pd
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.keys import Keys
import time
import os
import numpy as np
```


- Import your data as a CSV file that contains the list of SMILES

`df = pd.read_csv('FileName.csv')`
- ADMET2 only let you to analysis 500 component at the same time. Therefore, if you have more components you have to create a for loop and import your data with 500 intervals.

```python
df1=df.iloc[1:500]
search=list(df1.smiles)
```

- Adding the driver

```python
options = Options()
driver=webdriver.Chrome(options=options)
```
- Adding the website link

`driver.get("https://admetmesh.scbdd.com/service/screening/index")`

- selecting botton1

```python
element = WebDriverWait(driver, 20).until(EC.element_to_be_clickable(('xpath', "//a[@id='drawing-tab']")))
element.click()
```


- selecting search botton2

```python
x=WebDriverWait(driver, 20).until(EC.element_to_be_clickable(('xpath', "//textarea[@class='form-control']")))
for smiles in search:
	x.send_keys(smiles,"\n")
```

- click on the submite botton

```python
submit = WebDriverWait(driver, 20).until(EC.element_to_be_clickable(('xpath', "//button[@aria-label='Submit']")))
submit.click()
```


- doanloading the file

```python
dowanload = WebDriverWait(driver, 20).until(EC.element_to_be_clickable(('xpath', "/html/body/div/div[1]/div/div[2]/div/div[2]/a/button")))
dowanload.click()
time.sleep(20)
```
