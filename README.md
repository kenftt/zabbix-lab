# Zabbix Lab

## Description

The `zabbix-lab` project is designed to provide a lab environment for deploying and experimenting with Zabbix, an open-source network monitoring software. This lab leverages Vagrant to orchestrate the setup of virtual machines that host various components of Zabbix, including the Zabbix server, Zabbix agents, and a PostgreSQL database for data storage.

## Prerequisites

Before you can use this lab, ensure you have the following installed on your system:

- Vagrant: [Download & Install Vagrant](https://www.vagrantup.com/downloads)
- VirtualBox: [Download & Install VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- Necessary Vagrant plugins:
  - `vagrant plugin install vagrant-faster`
  - `vagrant plugin install vagrant-cachier`

## Installation

To set up the Zabbix lab environment, follow these steps:

1. Clone this repository to your local machine:

`git clone https://github.com/yourusername/zabbix-lab.git`

2. Navigate to the cloned repository directory:

`cd zabbix-lab`

3. Start the Vagrant environment:

`vagrant up`

This command will read the `Vagrantfile`, download the required box images, and provision the VMs as configured.

## Usage

Once the environment is set up, you can start interacting with the Zabbix server and agents. Here are some basic steps to get you started:

1. Access the Zabbix web interface by navigating to `http://192.168.56.101` (assuming `192.168.56.101` is the IP configured for the Zabbix server VM in your `Vagrantfile`).
2. Log in with the default Zabbix credentials (usually `Admin` for the username and `zabbix` for the password).
3. Begin configuring your monitoring environment by adding hosts, items, and triggers.

## Customization

You can customize the lab environment by editing the `Vagrantfile`. This includes changing the number of nodes, their configurations (CPUs, memory), and network settings.