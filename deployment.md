## Deployment

A catch-all category for cloud computing, data center provisioning, that
kind of stuff.

### Glossary

*   AWS - Amazon Web Services. One of the major cloud providers.

*   Azure - Microsoft's cloud. One of the major cloud providers.

*   Google Cloud (GCP) - One of the major cloud providers.

*   Terraform - Hashicorp's "infrastructure as code" tool for managing
    cloud compute resources.

*   Terraform Cloud -

*   Terraform Enterprise -

*   Infrastructure as code - You want to launch a web site with a
    database. You write a config file that says what you want your AWS
    setup to look like. You run a tool that makes reality reflect your
    config file. When it's done working, your web site is on the
    Internet.

    The idea, I guess, is that you can then manage this deployment
    information as you would any other code.

    Also it's declarative in the best sense. If you want to change
    something, you just patch the config file to say what you want. The
    tool then figures out the current actual state of affairs and
    decides how to get from here to the desired state.

*   Cloud provider - An organization that rents compute resources:
    machines and storage. You can rent virtual machines (to run an image
    of your choice on them) or (I think) some higher-level compute
    platform.

*   Docker

*   Container

*   Virtual machine

*   Virtual machine images

*   Availability zone

*   Region

*   Docker

*   Container

*   Kubernetes

*   OpenStack


### Questions

*   What's the word for the code that runs inside a container?

*   What kind of overhead do containers impose on the code inside?

    None in userspace, I think, unlike VMs which impose another level of
    MMU.

*   Is it compounded if the host Linux kernel is itself a guest VM instance?

*   How does Kubernetes compare to Docker? Do they have their own cloud
    services, "AWS for containers"?
