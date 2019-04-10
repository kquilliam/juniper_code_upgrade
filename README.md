# juniper_code_upgrade
Before I explain whats going on here, keep in mind that this is about a year old and could use a bit of cleaning up to gain some efficiency.
------
In my environment, I deal with a fair amount of low bandwith, high latency circuits that make the use of more readily available modules like `junos_package` or `juniper_junos_software` next to impossibe to use. It also makes trying to fit all of this into a single playbook hard since it can take 1.5 hours to push code to a box sometimes, while others are just a few seconds.

There are 2 playbooks here, each with their own distinct roles; Prestage and Code Upgrade
- Prestage -
This logs into a device, pulls the model info, locates the package on the control machine and SCP's it to the target device. It then verifies the file is there and writes the hostname to a file that can later be used for Slack notification.

- Code Upgrade -
This has some partial overlap with the prestage at the start with it logging into the device and verifying the model and that the code is present already. There is an assortment of RPCs and Jsnapy(pre/post/check) being done to account for various iterations of routing protocols because Jsnapy doesnt like Null values(fair). This playbook also utilizes Solarwinds Orion API to verify a node recovers after the install. Like the prestage, it sorts and dumps all the hostnames into seperate files that can be called up for Slack notifications.


Ive included a simple vars file called `model.yml` that lists all the variations of devices Ive encountered. I have also included the JSNAPY test files within `/jsnapy_test_files`, in my environment they are kept in `/opt/jsnapy/tests` but feel free to put them wherever and modify the playbook accordingly. The names are somewhat misleading as they can probably be utilized for more than SRXs but that is our primary device deployment so your mileage may vary.
