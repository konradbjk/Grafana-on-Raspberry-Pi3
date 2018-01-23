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

## Data Sources
Data sources allows you to import your data to Grafana. What is more if you want just to check grafana solution, there is possibility to use [test data](http://docs.grafana.org/features/datasources/testdata/). Data Sources can be added by clicking in the top left corner on the grafana logo. Next from dropdown menu choose *Data Sources*.

![data_sources](https://github.com/konradbjk/Grafana-on-Raspberry-Pi3/blob/master/graphics/data_sources.png).

In the next step click on *Add data source* button. Now you can choose type of  data source, for example MySQL like here.

![add_data_source](https://github.com/konradbjk/Grafana-on-Raspberry-Pi3/blob/master/graphics/add_data_source.png).

Depending on the source you need to feel different fiels.

### Import data from MySQL
Firstly you should name your data source, possibly something that is connected to the data located in the database. Next please choose MySQL as Type. In one of previous steps we did [install MySQL locally on Raspberry Pi](https://github.com/konradbjk/Grafana-on-Raspberry-Pi3/blob/master/docs/mysql_setup.md). So in this scenario the host field should be ```loaclhost:3306``` as it is default option, if you had changed it while installing MySQL use adequate port number. In the *Database* fill the name of Database you have created. Next fill the credentails of your grafana-reader account. You need to repeat the proccess for each of the databases you had created.

### AWS CloudWatch
If you have some EC2 instances on Amazon Web Services this one is perfect for you. It allows you to check state of instances without login to the console. You can ask for storage, cpu utilization and many more. Good for you that this data source is installed by default.
#### AWS
I recommend to create user who will only have permissions only to CloudWatch. What is more, this user should be able only to login by Access Key and Secret Access Key (progamatical access). Remember to keep both of them in secure place. [Here](https://github.com/konradbjk/Grafana-on-Raspberry-Pi3/blob/master/docs/AWS_policy_CloudWatch.txt) is AWS policy that I created for my user.

Full specification of CloudWatch is [here](http://docs.grafana.org/features/datasources/cloudwatch/).

## How to...
This section will provide quick guides how to do small improvments.

### Printing Data series
Having MySQL DB with table called Beacon Application we have multiple records there. This table structure is like below
![Structure of Beacon Applications MySQL](beacon_application_db_structure.png)
This table consist of values which are target ones for each of the months and the actual vale, which is updated on daily basis.
Next you have to create new Dashboard. Please select the Graph panel. In the General tab, change the name. Next navigate to Axes tab. Set in Left Y *Y-min* to *0* next uncheck Right Y and change X-axis mode to Series. Now go to Metrics tab. The best option is to create SQL querry for each of the columns separately (in our example it is 2 per graph, one for Target and one for Actual). Remember to choose the right data source.
```SQL
SELECT 
UNIX_TIMESTAMP(date_column) as time_sec, 
smart_beacon_actual as value,
"Smart Beacon actual" as metric
FROM BeaconApplications
ORDER BY date_column DESC
LIMIT 1
```
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

### Import videos
Similarly to importing photo, we will need to use html. You need to add Text panel and chose to edit it and in the Options tab change from markdown to html. Let's say that you want to import video from youtube. You can just click under the video to share it and choose *EMBED* option. Then you can add some interesting parameters or add them manually.
The most important is to chose to disable video description by setting ```showinfo=0```. Secondly you probably want the video to start automatically, then add ```autoplay=1```. It might be good if after the movie there will be no ads or suggested videos so also add ```rel=0```. There is high possibility also that you would like movie to be played in a loop, you can set this by adding parameter ```version=3&loop=1&playlist=VIDEO_ID``` where you need to replace VIDEO_ID with equivalent for you string. To add multple parameters manually at the end of the url ad question tag ```?``` and parameters. To add more then one you need to connect them with ```&``` sign. Final code shode look like below.
```
<iframe id="ytplayer" type="text/html" width="640" height="360"
  src="https://www.youtube.com/embed/M7lc1UVf-VE?version=3&loop=1&playlist=M7lc1UVf-VE&autoplay=1&rel=0&controls=0&showinfo=0"
  frameborder="0"></iframe>
```
You can also embed playlists or specific user videos. Check full [documentation of IFrame player API](https://developers.google.com/youtube/player_parameters).

## Usefull plugins


### Pie Chart

Full documentation of Pie Chart plugin is [here](https://grafana.com/plugins/grafana-piechart-panel).

### AJAX
AJAX = **A**synchronous **J**avaScript **A**nd **X**ML.
This panel allows you to load external content to you Dashboards. It is told to be better than just using the iframe objects.

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
Full documentation of Clock plugin is [here](https://grafana.com/plugins/grafana-clock-panel).

### Annunciator

Full documentation of Annunciator is [here](https://grafana.com/plugins/michaeldmoore-annunciator-panel).
