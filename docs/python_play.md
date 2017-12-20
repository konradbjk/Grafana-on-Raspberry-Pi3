# Python play

## API calls
To send api call we use **requests** or **urllinb**.
```python
import requests
import import urllib.request as urllib2
```
Now if we want to get simple GET request we just run. Let's say that our API url is assigned to ```xurl```
```python
response = request.get(xurl)
print(response.text)
```
We can set url with added parameters or add them separately.
```python
payload = { 'key1': 'value1', 'key2': 'value2'}
response = requests.get(url=xurl, params=payload)
```
You might want to check the url by
```python
print(response.url)
```
Playing around you might find that some of the text content of responses is wrongly encoded. To check encoding you can call
```python
print(response.encoding)
```
To change the encoding you simply assign value to it
```python
response.encoding = 'ISO-8859-1'
```
We can also call for binary response by simply calling
```python
print(response.content)
```
We can also add custom headers to the request for example
```python
 response = requests.get(
        url='https://api.getbase.com/v2/deals?value=10000.00&sort_by=created_at:desc&per_page=5',
        headers={
          'Accept': 'application/json',
          'Authorization': 'Bearer '+ basecrm_auth_token
        },
        verify=True
    )
```
All header values must be a string, bytestring, or unicode. While permitted, it's advised to avoid passing unicode header values.