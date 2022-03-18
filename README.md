# Auto tuning
IoTG Edge platforms like CoffeeLake, TigerLake offer various control knobs (Hardware, Hypervisor, Operating System) to suitably tune to achieve optimal performance.
Identifying the knobs and assigning the right values require high level of domain expertise or trial and error experiments including reboots for settings to take effect. The tuning values are dependent on the choice of workloads; hence a one-size fits all approach would not be effective.

## Solution
Leverage a suitable hyperparameter optimization techniques (like AutoML) along with an automation framework to converge on the correct values for tuning the knobs.
Platform Auto Tuning framework uses Optuna, an open source Hyperparameter optimization toolkit and enables the efficient searching of optimal tuning parameters within a limited number of cycles.
# Pre requisites

[Python](https://www.python.org/downloads/) version 3.8 and above

[OpenVINO for Linux](https://docs.openvinotoolkit.org/latest/openvino_docs_get_started_get_started_linux.html)

[Optuna](https://pypi.org/project/optuna/)
```
# pip install optuna
```
[Paramiko](https://pypi.org/project/paramiko/)

```
# pip install paramiko
```

# Installation

Go to $Install_Dir/web_console/web_page/ and run the following command to start the web Application
```
# python manage.py runserver
```
Then go to 127.0.0.1:8000 (Local_host:port) to get the web UI.

Make sure that you have the workload ready on the DUT before using this tool.

# How to run

### For new platform tuning

Fill the details and Click on 'Start Tuning', it will take some time based on the workload and shows the base performance, Optimized performance and percentage of change.
### Home Page preview
<img src="web_console/web_page/images/Home_page2.png" />

### Home Page preview after starting the process

<img src="web_console/web_page/images/Home2.png">

### Already had the tuned file?

Apply tuned file to any other machine

<img src="web_console/web_page/images/Had_tuned_file1.png" >

Loading of Page

<img src="web_console/web_page/images/Had_tuned_file2.png" >

#### Note : File should be present in  $Install_Dir/web_console/web_page/

### Error Page
For displaying any errors in the process. if all goes well, you can see the respective results page.
<img src="web_console/web_page/images/Error1.png" >

## Working of this tool

SSH from Paramiko is used to login to remote server and run the workload.

Then by using Optuna, we will create a study object and the we'll call optimize function from Optuna and passes the Objective function and number of trails as arguments to it.

## Files and details about it

#### Tuning file

Tuning.yaml :

Contains the parameters, type of parameters, range of values for each parameter.

  ##### Structure of Tuning file
  ```
-  Level:
    name of parameter:
      - Category (Only for OS level)
      - type of parameter
      - min and max if type is 'range' (or) list of values if type is 'values'
      - command to apply/change that parameter(Only for OS and Hardware)
  ```

  <img src="images/Tuning_File.png">


#### Config file
Config.yaml :

Contains the variables, test commands that are needed to be run to get the Objective(Ex : FPS for an inference application). Information from this file is used to make the commands that are required to run the application which will give the objective(ex: FPS).
##### Structure of Confing file

```
-  Command name:
    cmd : Command to execute in order to get the
    Parameters : Parameters are the variables that will be placed in the command. Variable names are kept inside {} in the cmd
    tune : List of tunables in that command
    result : keyword used to fetch the output(Say FPS for example) after executing the cmd.
```

<img src="web_console/web_page/images/Config_File.png">



### Results Page
<img src="web_console/web_page/images/Results.png">

### Result of applied tuned file page
<img src="web_console/web_page/images/Had_tuned_file3.png" >
