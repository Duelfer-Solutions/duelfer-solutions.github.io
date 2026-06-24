First found suggestion from canonical blog: [https://discourse.ubuntu.com/t/developing-with-ai-on-ubuntu/75299](https://discourse.ubuntu.com/t/developing-with-ai-on-ubuntu/75299)

Routed to tutorial on setting it up:  
[https://documentation.ubuntu.com/lxd/stable-5.21/tutorial/first\_steps/# first-steps](https://documentation.ubuntu.com/lxd/stable-5.21/tutorial/first_steps/#first-steps)

Need to decide between running container or VM:  
[https://documentation.ubuntu.com/lxd/stable-5.21/explanation/instances/# containers-and-vms](https://documentation.ubuntu.com/lxd/stable-5.21/explanation/instances/#containers-and-vms) 

- Likely just need container, because we don’t need functionality outside the OS of the “host machine” (i.e., my computer)

You can mount local files into the container:  
[https://documentation.ubuntu.com/lxd/stable-5.21/howto/instances\_access\_files/](https://documentation.ubuntu.com/lxd/stable-5.21/howto/instances_access_files/)

Full process:  
Create new linux container
```
lxc launch ubuntu:24.04 client1
```

List your containers
```
lxc list
```

Shell into your container
```
lxc shell client1
```

When in your container, simply exit to return to WSL
```
exit
```

Commands to stop and destroy containers, if necessary:
```
lxc stop client1  
lxc delete client1
```

# Client1 process  
```
lxc launch ubuntu:24.04 client1  
lxc shell client1  
curl -fsSL https://claude.ai/install.sh | bash  
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc
```

#  Install node manually for linux following  
[https://nodejs.org/en/download](https://nodejs.org/en/download)
```
# Download and install nvm:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.5/install.sh | bash

# in lieu of restarting the shell
\. "$HOME/.nvm/nvm.sh"

# Download and install Node.js:
nvm install 24

# Verify the Node.js version:
node -v # Should print "v24.18.0".

# Verify npm version:
npm -v # Should print "11.16.0".
```

#  open VS Code on your expected target folder that WILL BE mounted. You will be in the simple WSL setup right now
```
cd <your_folder>
code .
```

#  mount the file system  
```
lxc file mount client1
```

#  in a separate wsl terminal, access file system  
```
sshfs <user_name_from_mount_command>@127.0.0.1:/home/ubuntu/<your_folder> /home/<your_wsl_username>/client1 -p <port_from_mount_command>
```

#  Ensure a device is added so the host port can forward down to container:  
The port number will likely depend on the framework you are developing in (most have some target port)
```
lxc config device add client1 client1-5173 proxy listen=tcp:0.0.0.0:5173 connect=tcp:127.0.0.1:5173  
```
#  did the same on 8976 to support wrangler login  
lxc config device add client1 client1-8976 proxy listen=tcp:0.0.0.0:8976 connect=tcp:127.0.0.1:8976

# Cloudflare pages

[https://developers.cloudflare.com/pages/framework-guides/deploy-anything/](https://developers.cloudflare.com/pages/framework-guides/deploy-anything/)  
Not true anymore \- exit 0 as deploy command  
Use wrangler command with super simple config
