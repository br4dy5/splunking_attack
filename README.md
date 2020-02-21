# splunking att&ck
how to import the MITRE ATT&amp;CK Enterprise data for Splunking

this is the abreviated version to notate actions I just took in case I need to recreate them in the future. I plan to dummy them down even more in the future.

I'm sharing the configuration I chose, but these can be modified and still work. 

## Set up AWS environment
Helpful documentation: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html
### EC2 Instance
Size: t2.medium or greater recommended, but may be able to get away with micro free tier.
OS: I used Ubuntu 18.04, as that's what I'm comfortable with, but theoretically can use any OS Splunk supports

### Security Group
Make sure to tie down the inbound/outbound rules. Suggested to authenticate with key pair.

## Install Splunk
Helpful documentation: https://dev.splunk.com/enterprise/dev_license/
### Login to your new EC2 instance
`ssh -i "your_filename.pem" username@12.23.567.89`

### Download Splunk
`wget -O splunk-8.0.2-a7f645ddaf91-linux-2.6-amd64.deb 'https://www.splunk.com/bin/splunk/DownloadActivityServlet?architecture=x86_64&platform=linux&version=8.0.2&product=splunk&filename=splunk-8.0.2-a7f645ddaf91-linux-2.6-amd64.deb&wget=true'`

### Install Splunk
Helpful documentation: https://linoxide.com/linux-how-to/install-splunk-ubuntu/

`sudo dpkg -i splunk-8.0.2-a7f645ddaf91-linux-2.6-amd64.deb`
 
### Enable Boot Start
`cd /opt/splunk/bin`
`sudo ./splunk enable boot-start`
`sudo service splunk start`
<agree to things>

## Download ATT&CK data
`wget https://raw.githubusercontent.com/mitre/cti/master/enterprise-attack/enterprise-attack.json`
(this can be locally; does not need to be saved directly onto the server)

### Modify downloaded json
`cd <location of downloaded json>`
`nano enterprise-attack.json`
at the beginning of the file, delete the first open brace and bracket, aka the following:
  ```
 {
    "type": "bundle",
    "id": "bundle--83dad14b-ae53-4473-9f95-5ae37c8eaa5d",
    "spec_version": "2.0",
    "objects": [
 ```
 at the end of the file, delete the closing bracket and brace, aka the following:
```
 ]
}
 ```
why? because this is basically header info and will jack up how the data is ingested into Splunk
  
## Import the data
Login to your new Splunk console by going to the URL it gave you when you started Splunk. probably something like:
Waiting for web server at http://127.0.0.1:8000 to be available.. Done
Login using the Splunk Admin username/password you created at installation.
Once in the console, go to "Settings" from the top right menu, and select the icon labeled "Add Data"
Select the "Upload" icon from the bottom of the page
Click the "Select File" button and browse to the enterprise-attack.json file you previously downlaoded and modified.
Select "Next"
It should suggest \_json as the data type. Select "Next"
It's suggested to rename the "host" to include the data of the ATT&CK export for future reference. Maybe something like "enterprise-attack-DDMonYYYY"
It's also suggested to modify the Index, perhaps to "attack" by clicking the "Create a new index" link. On the next screen, the only field value you need to worry about is Index name. Click "Save"
Click "Review"
Click "Submit"



  
