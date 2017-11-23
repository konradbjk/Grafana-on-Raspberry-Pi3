# Grafana
There is used [unofficial version of Grafana](https://github.com/fg2it/grafana-on-raspberry) by [fg2it](https://github.com/fg2it) in this setup, mainly due to better optimalization to the ARM processors. To keep up to date with newest version of grafana I recommend to add Bintray Debian repository so that using apt we will be able to dowload it.
```
sudo apt-get install apt-transport-https curl
curl https://bintray.com/user/downloadSubjectPublicKey?username=bintray | sudo apt-key add -
echo "deb https://dl.bintray.com/fg2it/deb stretch main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```
After this we need to run following commands
```
sudo apt-get update
sudo apt-get install grafana
```
You can find more installation options checking the [repository wiki](https://github.com/fg2it/grafana-on-raspberry/wiki). After installing Grafana we need to set it automatically to start with our Raspberry Pi.
```bash
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable grafana-server
```


## Creating organisations
It is important to create different organisations. I will create accessible to everyone inside the company and one only for developers. Dashboard from the first one will be called in my example Kontakters and access to it can be provided to each of the employees. Also Dashboards from this organisation will be added to our Playlist.

## How to...
This section will provide quick guides how to do small improvments.
### Enable access to dashboards without need to login
Inside the *grafana.ini* file scroll down to find Anonymous Auth section and change as specified below (remember to delete the semicolon in front of each line you had edited)
```
# enable anonymous access
enabled = true

# specify organization name that should be used for unauthenticated users
org_name = Kontakters

# specify role for unauthenticated users
org_role = Viewer
```

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

Full specification of CloudWatch is [here](http://docs.grafana.org/features/datasources/cloudwatch/).

### Pie Chart

Full documentation of Pie Chart plugin is [here](https://grafana.com/plugins/grafana-piechart-panel).

### AJAX
This panel allows you to load external content to you Dashboards.

To install this plugin type in bash
```bash
sudo su
grafana-cli plugins install ryantxu-ajax-panel
service grafana-server restart
```

Full documentation of AJAX plugin is [here](https://grafana.com/plugins/ryantxu-ajax-panel).

### Clock
If you would like to present for example countdown statistics this is what you need. Product launch countodwn? Here is how to work this out.

To install this plugin type in bash
```bash
sudo su
grafana-cli plugins install grafana-clock-panel
service grafana-server restart
```
### Annunciator

Full documentation of Annunciator is [here](https://grafana.com/plugins/michaeldmoore-annunciator-panel).
