# Remotely Connect Multiple RDS Instances with phpMyAdmin

> I believe every System Admin working on Amazon Cloud might have faced a scenario where clients demand control of all the RDS databases via a single PHPMyAdmin host.

> Even I have been asked to do the same from one of my clients. Many of you  might be already familiar with this process, but if you are not; I have mentioned detailed step-by-step guidelines to Remotely Connect Multiple RDS Instances with phpMyAdmin.

> Let me explain the steps which helped me accomplish client’s requirement.

> Firstly, you need to install phpMyAdmin using below command:

```
sudo apt-get install phpmyadmin
```
> After doing the same you need to ensure whether the RDS instances are accessible from the phpMyAdmin host or not.

> To check the same, you need to run the below mentioned command from your phpMyAdmin host.

```
#mysql –h YourRDS Endpoint -P 3306 -u Username –pPassword
```

> In case the RDS instance is not accessible from the host you will receive an error like:
```
ERROR 2003 (HY000): Can’t connect to MySQL server on ‘Your RDS Endpoint‘ (110)
```
> If you do, then you need to check whether the Security Groups of your RDS instance has an entry of the phpmyadmin host’s CIDR/IP or Security Group associated with the host.

> Once you have made all these changes, you can now access the RDS instance from host.

> Now we need to make changes in phpMyAdminconfig file for Multiple RDS instances connections.

> An apt-get installation of phpMyAdmin by default is done under /etc/dir which can connect only to MySQL on localhost.

> Now in order to enable Multiple RDS instances to connect to single phpMyAdmin host, go to the

> config.inc.php file under /etc/phpmyadmin

> And search here for $cfg[‘Servers’][$i]

> After moving down few lines you will find a line $i++;

> After this line add following lines.

```
$cfg['Servers'][$i]['host'] = 'Your RDS Endpoint';
$cfg['Servers'][$i]['port'] = '3306';
$cfg['Servers'][$i]['socket'] = '';
$cfg['Servers'][$i]['connect_type'] = 'tcp';
$cfg['Servers'][$i]['extension'] = 'mysqli';
$cfg['Servers'][$i]['compress'] = TRUE;
$cfg['Servers'][$i]['auth_type'] = 'config';
$cfg['Servers'][$i]['user'] = 'USERNAME';
$cfg['Servers'][$i]['password'] = 'PASSWORD';
```
> For another RDS host entry add another entry below this & so on to connect more RDS hosts .

```
$i++;
$cfg['Servers'][$i]['host'] = 'Your RDS Endpoint';
$cfg['Servers'][$i]['port'] = '3306';
$cfg['Servers'][$i]['socket'] = '';
$cfg['Servers'][$i]['connect_type'] = 'tcp';
$cfg['Servers'][$i]['extension'] = 'mysqli';
$cfg['Servers'][$i]['compress'] = TRUE;
$cfg['Servers'][$i]['auth_type'] = 'config';
$cfg['Servers'][$i]['user'] = 'USERNAME';
$cfg['Servers'][$i]['password'] = 'PASSWORD';
```

> Now navigate to URL http://ElasticIPofyourhost/phpmyadmin

> On the main page you will see an entry Server Choice.

> Select the RDS instance you want to connect to, supply Username & Password of RDS instance & Get Connected !!!!

Enjoy!