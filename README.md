# Web Scraping in Python BeautifulSoup, Selenium & Scrapy 2023



https://requests.readthedocs.io/en/latest/user/quickstart/

### Customizing the Request Object

The `Request` object in the `urllib` library allows you to customize your HTTP requests by specifying different parameters. Here are some ways you can tailor the `Request` object to suit your needs:

1. URL: Set the `url` parameter to specify the target URL for your request. This determines the destination of the HTTP request.
2. Headers: Add headers to the request by passing a dictionary of header key-value pairs to the `headers` parameter. Headers contain additional information about the request, such as user-agent, content-type, authentication, and more.
3. Request Method: By default, the request method is set to GET. However, you can set the `method` parameter to specify a different HTTP method, such as POST, PUT, DELETE, etc.
4. Data: For requests that require sending data, such as form submissions, you can include the data as a dictionary or a string and pass it to the `data` parameter. This is useful when making POST requests or sending other types of data to the server.
5. Request timeout: You can set the `timeout` parameter to specify the maximum amount of time to wait for a response from the server. This is useful to prevent the request from hanging indefinitely if the server takes too long to respond.

By customizing the `Request` object with these parameters, you can tailor your HTTP requests to meet specific requirements and send them using the `urlopen` function to interact with web servers and retrieve the corresponding responses.

```python
request = urllib.request.Request(url=url, header=header)
response = urllib.request.urlopen(url)
```


The **main reason**s for using both the Request and urlopen classes from the urllib library together are as follows:

The Request class allows you to add headers to the HTTP request. Headers contain additional information about the request, such as user-agent, content-type, authentication, etc. By creating a Request object and passing the headers as an argument, you can customize the headers sent with the request. On the other hand, the urlopen function alone does not provide a straightforward way to add headers to the request.

The urlopen function is used to send the HTTP request and retrieve the server's response. It returns a response object that provides various methods and attributes to access information about the response. You can read the response content, check the status code, access the headers of the response, and perform other operations related to handling the response data.

Both Request and urlopen have different parameters and serve different purposes. The Request class is primarily used for constructing the request object with specific parameters, including the URL and headers. On the other hand, urlopen is used to actually send the request and receive the response from the server.

By combining the Request class and the urlopen function, you can customize the request with headers and then use urlopen to send the request and obtain the response, allowing you to have more control and flexibility in handling HTTP requests and responses.



### POST request

> In the HTTP protocol, there are two common request methods: GET and POST. GET requests typically attach parameters as a query string appended to the end of the URL, for example, `http://example.com/path?param1=value1&param2=value2`. On the other hand, POST request parameters are sent through the request body and are not directly appended to the URL.

**For POST requests, the  parameters must be encoded and placed in the request body.**  To achieve this, the code uses the `urllib.parse.urlencode()` method to URL-encode the parameters, converting them into a format that is safe for passing in a URL. Then, `.encode('utf-8')` is used to convert the encoded string into a byte stream for passing the parameters in a POST request.

Next, a `urllib.request.Request` object is created, with the URL, encoded parameters, and request headers passed as arguments to **customize the POST request.**

In summary, the code comments and the first answer highlight two important aspects of POST request parameters:

1. The parameters of a POST request are not directly appended to the URL but need to be placed in the parameters of the request object customization.
2. The parameters of a POST request must be encoded before being passed, ensuring their security and correct transmission. In this example, the `urllib.parse.urlencode()` method is used for URL encoding, followed by `.encode('gbk')` to convert it into a byte stream.

These explanations differ from the second answer, which did not include the step of parsing the JSON response from the server, but focused on explaining the encoding and placement of POST request parameters.

```python
import ssl
import urllib.request
import urllib.parse
ssl._create_default_https_context = ssl._create_unverified_context


url = 'https://translate.google.com/'

headers = {
'User-Agent':
'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36'
}

data = {
    'q': 'python'
}

# post リクエストパラメータはエンコードする必要があります
data = urllib.parse.urlencode(data).encode('utf-8')

# post request parameters need to be placed in the custom parameters of the request object, not appended after the url
# post request parameters need to be encoded
request = urllib.request.Request(url=url, data=data, headers=headers)

# ブラウザがサーバーにリクエストを送信する様子をシミュレートします。
response = urllib.request.urlopen(request)

# Get the response data
content = response.read().decode('utf-8')

# String --> json object

import json

obj = json.loads(content)
print(obj)


# The parameters of the post request method must be encoded data = urllib.parse.urlencode(data)
# After encoding, the encode method must be called data = urllib.parse.urlencode(data).encode('utf-8')
# The parameters are placed in the request object's custom method request = urllib.request.Request(url=url,data=data,headers=headers)

```



### Common request body encoding methods include:

In the HTTP protocol, a POST request typically requires encoding the request body as a byte stream. This is because POST requests are often used to send larger or more complex data, such as form data or JSON/XML formatted data. This data needs to be encoded in a certain format and transmitted to the server as a byte stream.

Common **request body encoding methods** include:

1. **application/x-www-form-urlencoded**: This is a common encoding method that encodes form data as key-value pairs, using an equal sign (=) to separate the key and value, and an ampersand (&) to separate different key-value pairs.
2. **multipart/form-data**: This encoding method is used for uploading files or sending form data that inclu des files. It divides the request body into multiple parts, each containing a file or form field, and separates them with a boundary delimiter.
3. **application/json**: If you are sending JSON-formatted data, you need to encode it as a byte stream and set the Content-Type header to application/json.

Please note that different programming languages and frameworks may provide specific libraries or methods to handle request body encoding. These libraries often handle the encoding details and convert it into a byte stream.

In summary, a POST request usually requires encoding the request body as a byte stream. The specific encoding method depends on the type of data you are sending and the server's requirements.



```python
import requests  # Importing the requests library to send HTTP requests
from bs4 import BeautifulSoup  # Importing the BeautifulSoup library for HTML parsing

website = 'https://subslikescript.com/movie/Titanic-120338'  # The URL of the website to fetch content from
result = requests.get(website)  # Sending a GET request to fetch the website's response
content = result.text  # Extracting the content from the response
soup = BeautifulSoup(content, 'lxml')  # Parsing the HTML content using BeautifulSoup with the lxml parser

```

Here is the explanation of the code:

1. Define a variable named `root` with the value `'https://subslikescript.com'`. This variable represents the root URL of a website.
2. Create a new variable named `website` using an f-string (`f'{root}/movies'`). This variable represents a specific URL of a website, combining the value of the `root` variable and the string `'/movies'`. The resulting URL will be `'https://subslikescript.com/movies'`.
3. Use the `requests` module's `get()` function to send a GET request to the aforementioned URL and retrieve the content of the webpage. This request will return a `Response` object, which is stored in the variable `result`.
4. Extract the content of the webpage from the `result` object and store it in the variable `content`. This can be done by accessing the `text` attribute of the `result` object, which contains the raw HTML content of the webpage.
5. Create a `BeautifulSoup` object named `soup` using the constructor function of the `BeautifulSoup` library. This object is used to parse the HTML content and provides easy access to elements and attributes within it. In this example, the HTML content is converted to a `BeautifulSoup` object using the `'lxml'` parser.

Summary: The purpose of this code is to use the `requests` and `BeautifulSoup` libraries to retrieve HTML content from a specific website and parse it using a `BeautifulSoup` object for further processing.

```python
box = soup.find('articles', class_='main-article')

links = []
for link in box.find_all('a', href=True):
    links.append(link['href'])
```

The purpose of this code is to find all the links within a selected HTML element (represented by the variable `box`) that have an `href` attribute, and store these links in the `links` list.

1. Create an empty list called `links` to store the links.
2. Use `box.find_all('a', href=True)` to find all the `<a>` tags within the HTML element `box` that have an `href` attribute. This function returns a list containing all the elements that meet this condition.
3. Enter a loop to iterate through each `<a>` tag that meets the condition:
   - Use `link['href']` to retrieve the value of the `href` attribute of the `<a>` tag, which represents the URL of the link.
   - Append the URL of the link to the `links` list.
4. After the loop finishes executing, the `links` list will contain all the found links.
5. If desired, you can use `print(links)` to print the content of the `links` list and check if the links were successfully stored.

Summary: The purpose of this code is to extract all the links from a specific HTML element and store them in a list for further processing.
