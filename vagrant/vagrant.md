# Vagrant cheat sheet

Typing `vagrant` from the command line will display a list of all available commands.

Be sure that you are in the same directory as the Vagrantfile when running these commands!

## Creating a VM

| Command                  | Discription                                                                                                                                                                         |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `vagrant init`           | Initialize Vagrant with a Vagrantfile and ./.vagrant directory, using no specified base image. Before you can do vagrant up, you'll need to specify a base image in the Vagrantfile |
| `vagrant init <boxpath>` | -- Initialize Vagrant with a specific box. To find a box, go to the [public Vagrant box catalog](https://app.vagrantup.com/boxes/search)                                            |

---

## Starting a VM

| Command                      | Discription                                                               |
| ---------------------------- | ------------------------------------------------------------------------- |
| `vagrant up`                 | Starts vagrant environment (also provisions only on the FIRST vagrant up) |
| `vagrant resume`             | Resume a suspended machine (vagrant up works just fine for this as well)  |
| `vagrant provision`          | Forces reprovisioning of the vagrant machine                              |
| `vagrant reload`             | Restarts vagrant machine, loads new Vagrantfile configuration             |
| `vagrant reload --provision` | Restart the virtual machine and force provisioning                        |

---

## Getting into a VM

| Command                 | Discription                                                                                                 |
| ----------------------- | ----------------------------------------------------------------------------------------------------------- |
| `vagrant ssh`           | Connects to machine via SSH                                                                                 |
| `vagrant ssh <boxname>` | If you give your box a name in your Vagrantfile, you can ssh into it with boxname. Works from any directory |

---

## Stopping a VM

| Command           | Discription                                  |
| ----------------- | -------------------------------------------- |
| `vagrant halt`    | Stops the vagrant machine                    |
| `vagrant suspend` | Suspends a virtual machine (remembers state) |

---

## Cleaning Up a VM

| Command              | Discription                                         |
| -------------------- | --------------------------------------------------- |
| `vagrant destroy`    | Stops and deletes all traces of the vagrant machine |
| `vagrant destroy -f` | Same as above, without confirmation                 |

---

## Boxes

| Command                        | Discription                                         |
| ------------------------------ | --------------------------------------------------- |
| `vagrant box list`             | See a list of all installed boxes on your computer  |
| `vagrant box add <name> <url>` | Download a box image to your computer               |
| `vagrant box outdated`         | Check for updates vagrant box update                |
| `vagrant box remove <name>`    | Deletes a box from the machine                      |
| `vagrant package`              | Packages a running virtualbox env in a reusable box |

---

## Saving Progress

| Command                                            | Discription                                                                             |
| -------------------------------------------------- | --------------------------------------------------------------------------------------- |
| `vagrant snapshot save [options] [vm-name] <name>` | vm-name is often `default`. Allows to save so that rollback at a later time is possible |

---

## Tips

| Command                                       | Discription                                                          |
| --------------------------------------------- | -------------------------------------------------------------------- |
| `vagrant -v`                                  | Get the vagrant version                                              |
| `vagrant status`                              | Outputs status of the vagrant machine                                |
| `vagrant global-status`                       | Outputs status of all vagrant machines                               |
| `vagrant global-status --prune`               | Same as above, but prunes invalid entries                            |
| `vagrant provision --debug`                   | Use the debug flag to increase the verbosity of the output           |
| `vagrant up --provision \| tee provision.log` | Runs `vagrant up`, forces provisioning and logs all output to a file |

---

## Notes

- [Hashicorp - Vagrant](https://developer.hashicorp.com/vagrant])
