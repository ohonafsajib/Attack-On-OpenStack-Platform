<!-- ABOUT THE PROJECT -->
## About The Project 

Modern computing is about protecting the complex digital ecosystems we navigate as well as executing daily activities with efficiency. The primary aim of the project revolves around the implementation of the cloud platform "OpenStack" and subsequently executing a network based attack within this virtual environment. We explore OpenStack to understand why it's important and how it works. We also investigate the basics of Snort IDS (a system for detecting intrusions) to understand how it works too.

A platform called OpenStack utilises shared virtual machines to create and administer private as well as public clouds. The "projects" or tools which make up the OpenStack architecture manage the core cloud computing functions of computations, storage, networking, identity, and image. In virtualization, a hypervisor divides services like archive, CPU, and RAM and abstracts them from multiple vendor specific applications before delivering them as needed.

## Project Objective 

1. Install OpenStack and deploy a virtual network
	-	Install OpenStack on a local computer, and use it to set up a small virtual network with at least 2 nodes
	-	Keep track of all the details/challenges/failures/solutions
2. Launch and detect a network-based attack
	-	The only requirement for the attack is that it must be network-based
	-	It must be launched from one node and target another node of your virtual network
	-	Can use any existing exploit code
	
## Built With

This section should list any major frameworks/libraries used to bootstrap your project. Leave any add-ons/plugins for the acknowledgements section. Here are a few examples.

* [![Next][Next.js]][Next-url]
* [![Snort][Snort.js]][Snort-url]
* [![Kali][Kali.js]][Kali-url]


<!-- GETTING STARTED -->
## What is Snort?

Snort is widely recognized and open-source Intrusion Detection System (IDS) software that plays a pivotal role in monitoring network traffic and identifying potential security breaches, anomalies, or malicious activities within computer networks. Snort examines data packets traveling through a network and uses a combination of signature-based detection, protocol analysis, and anomaly detection techniques to raise alerts and offer details about potential threats, enhancing the overall security and robustness of network infrastructures. 

![img](/images/snort.png)

## Network Based Attack and Detection using Snort

We conducted a DoS (Denial of Service) attack, which aimed to compromise the security of a targeted instance. To conduct this attack, we used the hping3 tool, known for its proficiency in generating such attacks. Our aim is to detect any kind of intrusion and possible network-based attack. By integrating the Snort Intrusion Detection System (IDS), we were able to actively identify and monitor the incoming attack traffic. Snort proved its effectiveness by detecting each ICMP ping echo request and echo reply.

## Technological Stack

To perform the project we have used a physical host machine (Ubuntu 22.04 LTS) and on top of the bare metal machine, we installed OpenStack. Technical Stack details has been given below 

![img](/images/tech_stack.png)

### Installation

These are the steps to install OpenStack with credential for Admin, Database, Rabbit and Services of OpenStack. Below image shows our OpenStack dashboard which shows Number of Instances, Allocation of VCPUs, RAM, Volume allocation, and Network utilities such as Security groups, Security rules, Ports, Routers, etc.

_Steps to install the platform is shared below -_

   ```sh
    sudo useradd -s /bin/bash -d /opt/stack -m stack
    sudo chmod +x /opt/stack
    echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
    sudo -u stack -i
    cd /opt/stack
    git clone https://opendev.org/openstack/devstack
    cd devstack
    vim /opt/stack/devstack/local.conf
    [[local|localrc]]
    ADMIN_PASSWORD=secret
    DATABASE_PASSWORD=$ADMIN_PASSWORD
    RABBIT_PASSWORD=$ADMIN_PASSWORD
    SERVICE_PASSWORD=$ADMIN_PASSWORD
    sudo grep -v ^# /opt/stack/devstack/local.conf
    ./stack.sh
   ```

<!-- USAGE EXAMPLES -->

## Operational Overview

This image shows the network topology with router making connection of public network with shared network (Internal Network) and such mapping helped us to access the instances outside the network. We can see details of these public networks and shared network in below image. We can see any interface that we add are shown here and it acts like a virtual router and do the routing between our nodes. We can create the security groups in Network topology section and apply any security policy that we need to. We can see the interface setting and set/edit the gateway IP.

We have three OpenStack images as per shown in below Image. In OpenStack, an "image" refers to a pre-configured and often virtualized operating system environment that serves as a template for creating virtual machines (VMs) or instances. These images encapsulate the necessary components, including the operating system, software, configurations, and even data, into a single file. When we want to create a new virtual machine in an OpenStack environment, we can choose an image that matches our desired specifications, such as the operating system type and version. By default, we have one image of CirrOS which is used for testing purposes. It is lightweight and efficient Linux distribution, so it is ideal choice due to its small size and rapid boot time.

## Methodology

In our project, we're working with two instances: Application_node_1, which serves as the attacker, and Target_node_01, our designated victim. To carry out the attacks, we've set up hping3 on Application_node_01. On the defensive side, we've equipped Target_node_01 with Snort, a specialized tool to detect and respond to potential threats. We can see resources information of both the instances in following images. 

## Attack and Detection

In the following step, we sent single ping request to ensure that out IDS is correctly detecting network activity of our network cluster.
On the right side, we executed the hping3 command for the single ping request, while on the left side, the Snort defense tool showcased its capability. It successfully identified that the target node, the victim in our network, received an ICMP ping echo request dispatched by the attacker. Furthermore, Snort promptly responded by transmitting an ICMP ping echo reply to the source.

## Types of attacks

We have tried our attack with hping3 with three different options --fast, --faster and --flood. Each option dictates a specific rate of packet transmission:

* --fast: it will send 10 packets per second.
* --faster: it will send 100 packets per second.
* --flood: it will send packets as fast as possible.

Here in the following three images, we are sending multiple ICMP ping requests to the victim at different speed rate and Snort is successfully detecting those requests and replying to that ICMP ping requests back to the sources.

Using the "--fast" and "--faster" flags can intensify the traffic load within the network, leading to a noticeable rise in the time taken for client responses. Conversely, opting for the "--flood" flag initiates an ongoing stream of packets, persistently bombarding the system until it becomes overwhelmed, resulting in an inability to effectively manage incoming requests. This can significantly impact the system's overall performance.

![img](/images/attack01.png)
![img](/images/attack02.png)
![img](/images/attack03.png)
![img](/images/attack04.png)

## Challenges & Overcomes

Initially, we tried to install OpenStack on virtual machine but due to Hypervisor Type-2 complexity, we are unable to established network mapping with shared network [internal network of OpenStack/ local IP]. Hypervisor Type-2 Complexities we had faced during the installation process are given below. 

* Provision instances (OpenStack VMs) on top of Virtual Platform
* Mapping network blocks
* Routing issues
* Processing delay

To avoid network complexity in hypervisor type-2, we did perform dual boot on our laptop with 10GB RAM, 50GB SSD, corei7 9th generation on Ubuntu 22.04 LTS. By doing such dual boot, we avoid Hypervisor dependency; we successfully install OpenStack and implemented one attack on it and at the same time the original workstation of our classmate (which was on Windows OS remained intact). 

Due to scarcity of resources, we used single node of architecture instead of multi node architecture and we install IDS on the same instance of victim machine rather than making third instance to install IDS.



<!-- 
## Usage

Use this space to show useful examples of how a project can be used. Additional screenshots, code examples and demos work well in this space. You may also link to more resources.

_For more examples, please refer to the [Documentation](https://example.com)_

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ROADMAP -->
## Roadmap

- [x] Add Changelog
- [x] Add back to top links
- [ ] Add Additional Templates w/ Examples
- [ ] Add "components" document to easily copy & paste sections of the readme
- [ ] Multi-language Support
    - [ ] Chinese
    - [ ] Spanish

See the [open issues](https://github.com/othneildrew/Best-README-Template/issues) for a full list of proposed features (and known issues).

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTACT -->
## Contact

Your Name - [@your_twitter](https://twitter.com/your_username) - email@example.com

Project Link: [https://github.com/your_username/repo_name](https://github.com/your_username/repo_name)

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

Use this space to list resources you find helpful and would like to give credit to. I've included a few of my favorites to kick things off!

* [Choose an Open Source License](https://choosealicense.com)
* [GitHub Emoji Cheat Sheet](https://www.webpagefx.com/tools/emoji-cheat-sheet)
* [Malven's Flexbox Cheatsheet](https://flexbox.malven.co/)
* [Malven's Grid Cheatsheet](https://grid.malven.co/)
* [Img Shields](https://shields.io)
* [GitHub Pages](https://pages.github.com)
* [Font Awesome](https://fontawesome.com)
* [React Icons](https://react-icons.github.io/react-icons/search)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

-->

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/othneildrew/Best-README-Template.svg?style=for-the-badge
[contributors-url]: https://github.com/othneildrew/Best-README-Template/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/othneildrew/Best-README-Template.svg?style=for-the-badge
[forks-url]: https://github.com/othneildrew/Best-README-Template/network/members
[stars-shield]: https://img.shields.io/github/stars/othneildrew/Best-README-Template.svg?style=for-the-badge
[stars-url]: https://github.com/othneildrew/Best-README-Template/stargazers
[issues-shield]: https://img.shields.io/github/issues/othneildrew/Best-README-Template.svg?style=for-the-badge
[issues-url]: https://github.com/othneildrew/Best-README-Template/issues
[license-shield]: https://img.shields.io/github/license/othneildrew/Best-README-Template.svg?style=for-the-badge
[license-url]: https://github.com/othneildrew/Best-README-Template/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/othneildrew
[Next.js]: https://img.shields.io/badge/OpenStack-DevStack-blue
[Next-url]: https://quillbot.com/
[Snort.js]: https://img.shields.io/badge/SNORT-IDS-ash
[Snort-url]: https://www.snort.org/
[Kali.js]: https://img.shields.io/badge/Kali-Linux-red
[Kali-url]: https://www.kali.org/
