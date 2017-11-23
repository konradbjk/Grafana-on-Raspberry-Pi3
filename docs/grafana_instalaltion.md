# Grafana

## Creating organisations
It is important to create different organisations. I will create accessible to everyone inside the company and one only for developers. Dashboard from the first one will be


## How to...

### Import photo
To do so you need to use html. Add new Text panel and click on its name to choose edit option. Go to the options tab and change mode to *html*. Let's say that you want to have photo of a cat. I have one from [Unsplash](www.unsplash.com). at [this address](https://images.unsplash.com/photo-1494256997604-768d1f608cac?auto=format&fit=crop&w=1101&q=60&ixid=dW5zcGxhc2guY29tOzs7Ozs%3D). We need to think also about possibility that our photo may not be accessible at some time, then we need to use parameter ```alt```. Below is my example, while the photo is not accessible it will say *error*
```html
<img src="https://goo.gl/vZ3sxu" alt="error" />
```

## Usefull plugins

### CloudWatch
If you have some EC2 instances on Amazon Web Services this one is perfect for you. It allows you to check state of instances without login to the console. You can ask for storage, cpu utilization and many more. Good for you that this plugin is installed by default.
#### AWS
I recommend to create user who will only have permissions only to CloudWatch. What is more, this user should be able only to login by Access Key and Secret Access Key (progamatical access). Remember to keep both of them in secure place. [Here](https://github.com/konradbjk/Grafana-on-Raspberry-Pi3/blob/master/docs/AWS_policy_CloudWatch.txt) is AWS policy that I created for my user.

Full specification of the plugin is [here](http://docs.grafana.org/features/datasources/cloudwatch/)

### Clock
If you would like to present for example countdown statistics this is what you need. Product launch countodwn? Here is how to work this out.

To install this plugin type in bash
```bash
sudo su
grafana-cli plugins install grafana-clock-panel
service grafana-server restart
```
