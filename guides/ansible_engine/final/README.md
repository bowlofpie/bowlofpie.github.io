# Final Exercise - Maintaining Desired State

Now we have written our first ansible *playbook*, we will run it and see the results. Then we will make some out-of-band changes and use ansible to detect and remediate them. 

## Section 1: Run the playbook

Let's run the playbook to install, configure and start the web server:

```bash
[student1@ansible files]$ cd ~/apache-simple-playbook

[student1@ansible apache-simple-playbook]$ ansible-playbook ./site.yml
```

Your output should look similar to this:

```bash
PLAY [Ensure apache is installed and started] *********************************

TASK [Gathering Facts] ********************************************************
ok: [node3]
ok: [node1]
ok: [node2]

TASK [Ensure httpd package is present] ****************************************
changed: [node2]
changed: [node3]
changed: [node1]

TASK [Ensure latest httpd.conf file is present] *******************************
changed: [node3]
changed: [node2]
changed: [node1]

TASK [Ensure httpd is started] ************************************************
changed: [node3]
changed: [node1]
changed: [node2]

PLAY RECAP ********************************************************************
node1                      : ok=4    changed=3    unreachable=0    failed=0   
node2                      : ok=4    changed=3    unreachable=0    failed=0   
node3                      : ok=4    changed=3    unreachable=0    failed=0   

```

Notice that the nodes are "changed" as we are making several changes to the servers.

Re-run your playbook again, and notice that no changes are made as the state has not changed and nothing needs to be done.


## Section 2: View the web site

First, we need to get the IP address of one of the web servers from the inventory file:

```bash
[student1@ansible apache-simple-playbook]$ cat ~/lightbulb/lessons/lab_inventory/<YOUR STUDENT ID>-instances.txt 
```


Make a note of the IP address of `node1` in the `web` inventory group.

Open your web browser and go to this IP address. You will see your web site up and running.


## Section 3: Make an unathorised change

Now we will make an unathorised change to the `node1` web site.

Using the `node1` IP address, log in to it using your student ID and password:

```bash
[student1@ansible apache-simple-playbook]$ ssh <YOUR STUDENT ID>@<IP ADDRESS OF node1>
```

Change the contents of the `index.html` file:

```bash
[student1@node1 ~]$ echo '<html><br><br><br><h1>You are hacked!</h1></html>' | sudo tee /var/www/html/index.html

```

Now refresh your web browser and see the changed site.

Logout of `node1`


## Section 4: Remediation

Now we will use our playbook to detect and remediate the change.

Re-run your playbook:

```bash
[student1@ansible apache-simple-playbook]$ ansible-playbook ./site.yml
```

Your output should look similar to this:

```bash
PLAY [Ensure apache is installed and started] *********************************

TASK [Gathering Facts] ********************************************************
ok: [node1]
ok: [node2]
ok: [node3]

TASK [Ensure httpd package is present] ****************************************
ok: [node1]
ok: [node3]
ok: [node2]

TASK [Ensure latest httpd.conf file is present] *******************************
ok: [node3]
changed: [node1]
ok: [node2]

TASK [Ensure httpd is started] ************************************************
ok: [node1]
ok: [node3]
ok: [node2]

PLAY RECAP ********************************************************************
node1                      : ok=4    changed=1    unreachable=0    failed=0   
node2                      : ok=4    changed=0    unreachable=0    failed=0   
node3                      : ok=4    changed=0    unreachable=0    failed=0   
```

Notice that `node1` has changed.

Refresh your web browser and see that the site content has been restored.

