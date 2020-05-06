# Working with remote processes

This document will explain a couple of ways to work with remote servers and databases. The solution you choose will depend on the exact way your infrastructure is set up, so this is just meant to explore a couple of tricks:


# 1. Work with remote files via SSH

We saw how we could SSH into the machine and use nano to edit files. But nano is not really the most powerful editor!

Luckily, many editors have the ability to work directly with remote files over SSH. For example, this is provided with the "Remote - SSH" extension for VSCode, "remote-edit" for Atom, or tramp in emacs.

Try installing setting up this extension in your editor and connecting to the remote EC2 instance! Remember that you will need to use the `.pem` file to connect!


# 2. Connect to a remote database from your local machine

Rather than working entirely within the remote server (working on remote files via SSH), this allows you to work with local files (`.sql` files and `.py` files and a local jupyter notebook) and connect to just the database running remotely on AWS.

This is a common situation if the server is dedicated to just running the database and other processes shouldn't be run on it.

To do this, we need two things:

1. We can make network connections (think HTTP requests) from our local machine to the database server. This either means that the database is available on the public internet or that we use SSH port forwarding.

2. We have a local database client software that can communicate with the database.


### SSH port forwarding

SSH port forwarding allows us to use the secure (encrypted) SSH connection to connect to a database (or server of any kind) running on a remote server.


``` shell
ssh -i mykey.pem -L 5432:localhost:5444 [USER]@[DNS]
```

Where `[DNS]` is the **Public DNS** of your AWS EC2 instance (you can also replace this with the **Public IP** address of the instance), `[USER]` might be `ubuntu` if you launched an Ubuntu instance on AWS, and `mykey.pem` is your local path to the private key file of the keypair used to launch the instance.

This allows you to connect to `locahost:5432` on your local machine and your connection will be "forwarded" to `localhost:5444` on the remote machine.

We can also use SSH port forwarding to securely connect to our Jupyter Notebook, without needing to expose `8888` via the firewall (security group) in AWS:

``` shell
ssh -i mykey.pem -L 8888:localhost:8888 [USER]@[DNS]
```

This is an easy way to have an encrypted connection to your remote notebook and avoid messing with security groups at the same time!


### Install a local client

You can now connect to the database (Postgres) server running on your EC2 instance FROM your local computer!

But, of course, you will need to install a client on your local computer.

Options:

1. psql (Postgres CLI client we used from the server).
2. pgadmin.
3. dbeaver.
4. Postgres extension in VSCode.
5. psql in postgres docker image (and docker installed locally).
6. Just do everything through a python client like `psycopg2`.
