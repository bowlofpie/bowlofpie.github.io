# Final Exercise - Maintaining Desired State

Now that you've gotten a sense of how ansible works, we are going to write our first ansible *playbook*. Playbooks are Ansible’s configuration, deployment, and orchestration language. They are written in YAML and are used to invoke Ansible modules to perform tasks that are executed sequentially i.e top to bottom. They can describe a policy you want your remote systems to enforce, or a set of steps in a general IT workflow. Playbooks are like an instruction manual and describe the state of environment.

A playbook can have multiple plays and a play can have one or multiple tasks.  The goal of a *play* is to map a group of hosts.  The goal of a *task* is to implement modules against those hosts.

For our first playbook, we are only going to write one play and two tasks.


## Section 1: Run the playbook

Let's run the playbook to install the web server:

```bash
[student1@ansible files]$ cd ~/apache-simple-playbook

[student1@ansible apache-simple-playbook]$ ansible-playbook ./site.yml
```

Your output should look similar to this:

```bash
PLAY [Ensure apache is installed and started] **************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************
ok: [node3]
ok: [node1]
ok: [node2]

TASK [Ensure httpd package is present] *********************************************************************************************
changed: [node2]
changed: [node3]
changed: [node1]

TASK [Ensure latest httpd.conf file is present] ************************************************************************************
changed: [node3]
changed: [node2]
changed: [node1]

TASK [Ensure httpd is started] *****************************************************************************************************
changed: [node3]
changed: [node1]
changed: [node2]

PLAY RECAP *************************************************************************************************************************
node1                      : ok=4    changed=3    unreachable=0    failed=0   
node2                      : ok=4    changed=3    unreachable=0    failed=0   
node3                      : ok=4    changed=3    unreachable=0    failed=0   

```

Notice that the nodes are "changed" as we are making several changes to the servers.


## Section 2: View the web site

To view the web site, first we need to get the IP address of one of the web servers from the inventory file:

```bash
cat  ~/zz
```


Make a note of the IP address of `node1` in the `web` inventory group.

Open your web browser and go to this IP address. You will see your web site up and running.


## Section 3: View the web site

First, we need to get the IP address of one of the web servers from the inventory file:

```bash
[student1@ansible apache-simple-playbook]$ cat ~/lightbulb/lessons/lab_inventory/<YOUR STUDENT ID>-instances.txt 
```


Make a note of the IP address of `node1` in the `web` inventory group.

Open your web browser and go to this IP address. You will see your web site up and running.


## Section 4: D

Now we will make an unathorised change to the `node1` web site.

Using the `node1` IP address, log in to it using your student ID and password:

```bash
[student1@ansible apache-simple-playbook]$ ssh <YOUR STUDENT ID>student1@13.229.211.65
```

Change the contents of the `index.html` file:

```bash
[student1@node1 ~]$ echo '<html><br><br><br><h1>You are hacked!</h1></html>' | sudo tee /var/www/html/index.html

```

Now refresh your web browser and see the changed site.

Logout of `node1`


## Section 5: R

Now that we've defined your play, let's add some tasks to get some things done.  Align (vertically) the *t* in `task` with the *b* `become`. Yes, it does actually matter.  In fact, you should make sure all of your playbook statements are aligned in the way shown here. If you want to see the entire playbook for reference, skip to the bottom of this exercise.

Re-run your playbook:

```bash
sss
```


Notice that `node1` has changed.

Refresh your web browser and see that the site content has been restored.

