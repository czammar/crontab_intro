# Crontab in GNU/Linux

# 0. Intro

Crontab is a Unix-like system utility to help us to execute jobs periodical and automatically. We present quick configuration for executing peridoc task with an Ubuntu based Linux distro.

# 1. Set up

```bash
# Set up
sudo apt-get update
sudo apt-get install -y cron nano
```

Optional:

```bash
# Crontab uses variable EDITOR to define a shell text editor
# and since Vim is ugly as hell (╯°□°）╯︵ ┻━┻, so we use nano
export EDITOR="nano"
```

**Note:** You can make it permanent adding the last command to .bashrc file of your profile and then use command "source .bashrc".

# 2. Basic use and sintaxis



## 2.1 Basics

Crontab works by editing some text file from our machine (typically /etc/crontab, but can also uses /etc/cron.d). Basically, we have to create a bash script for Crontab using command:

```bash
crontab -e
```

This will open our text editor in terminal and let us introduce command by following the next structure that specifies at what moment the job will be executed:

```bash
.---------------- minute (0 - 59) 
|  .------------- hour (0 - 23)
|  |  .---------- day of month (1 - 31)
|  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ... 
|  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7)  OR sun,mon,tue,wed,thu,fri,sat 
|  |  |  |  |
*  *  *  *  *  command to be executed
```

Note: 

+ After saving changes in you text editor and after that cron will begin with the automatic executions.
+ Crontab it dependens on internal clock of your Unix-like machine to execute the jobs periodically.

**More fun with Crontab**

- Asterisk (`*`) in a field signifies the entire range for that field (e.g. `0-59` for the minute field).
- A comma (`,`) can be used to specify a list e.g 1,4,6,8 (run job at minutes 1,4,6,8)
- Ranges are specified with a dash (`-`), for example run a job between 1 and 3.
- Ranges can be combined with lists e.g. 1-3,9-12 which means run a job in minutes between 1 and 3 then between 9 and 12.
- The `/` character can be used to introduce a step e.g. 2/5 which means starting at 2 then every 5 (2,7,12,17,22...). They do not wrap past the end.
- Ranges and steps can be combined e.g. `*/2` signifies starting at the minimum for the relevant field then every 2 e.g. 0 for minutes( 0,2...58), 1 for months (1,3 ... 11) etc.

### 2.1.1 Examples

*Run each minute to write a bash command ouptput to a text*

```bash
* * * * * echo "I was ran in this date: $(date)" >> /my_path/example.txt
```

*Run each 10 minutes*

```bash
*/10 * * * * echo "I was ran in this date: $(date)" >> /my_path/example.txt
```

*Run all days at 4:30 pm*

```bash
30 4 * * * echo "I was ran in this date: $(date)" >> /my_path/example.txt
```

*Run all days at 4:30 pm* but in business days (not weekends)

```bash
30 4 * * 1-5 echo "I was ran in this date: $(date)" >> /my_path/example.txt
```

## 2.2 More features

*Access crontab configuration of and specific user*

```bash
crontab -u my_username -e
```

*List all current crontab jobs*

```bash
crontab -l
```

*Eliminate current script of crontab jobs (be careful)*

```bash
contrab -r
```



## 2.3 Paths and execution on bash terminal

Please note that in Unix systems we can locate a program file in the user's path using *which* command. For example, we can find related to Python and Rscript (a bash utility to run R commands and scripts):

```bash
which python 
# /Users/my_user/.pyenv/shims/python

which Rscript
# /Users/my_user/opt/anaconda3/bin/Rscript
```

With this information, we can execute Python adn R file from terminal in two ways:

**Route 1: specifing paths on terminal**

You can help to execute a Python/R script by specifying the path where they are located, 

```bash
/my_path_to_python hello_world.py # python
```

```bash
/my_path_to_Rscript hello_world.R # R files, using Rscript
```

*Example:*

```bash
/Users/my_user/.pyenv/shims/python hello_world.py
```

**Route 2: shebang notation #!** 

We can add a header to out script to tell bash where to find the program that will execute a script, as follows:

```
#!//my_path_to_Rscript
...
# Some cool code
# More of that cool code
```

After that, we have to gave the script permision of execution:

```
chmod +x my_script.R
```

And run the script:

```
./my_script.R
```

## 2.4 Python/R scripts and Crontab

To execute a Crontab job for Python o R, we have to combine the previous knowledge:

**Example 1**

*my_example.py*

```
#!/my_path_to_Rscript
print("Hello Brenda")
```

**Open crontab job editor**

```
crontab -e
```

**Specify the right sintaxis of crontab**

```
*/10 * * * * /my_path_to_python my_example.py # Execute the script each 10 minutes
```



**Example 2**

*my_example.R*

```R
#!/my_path_to_Rscript
HelloBrenda <- function(){
   print('hello')
}

HelloBrenda()
```

```
chmod +x my_example.R
```

**Open crontab job editor**

```
crontab -e
```

**Specify the right sintaxis of crontab**

```
*/10 * * * * ./my_example.R # Execute the script each 10 minutes
```

# 3. References

+ https://help.ubuntu.com/community/CronHowto

+ Consulting crontab manual:

  ```bash
  man crontab
  ```

  