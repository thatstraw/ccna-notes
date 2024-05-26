- What is a Networking Model - Networking models categorize and provides structure for networking standards and protocols.
- A networking protocol is a set of logical rules that defines how network devices and softwares should operate.

## OSI Model

- OSI Model stands for Open Systems Interconnection Model.
- A conceptual model that categorizes and standardizes the different functions in a network.
- It was created by the “International Organization for Standardization” ISO
- Network Functions are devided into 7 layers
- These layers work together to make the network work properly

### OSI Model Layers

| Layer Number | Layer Name | Explanation &Functions |
| --- | --- | --- |
| 7 | Application | - This layer is the closest to the end user
- Interacts with application softwares for example your web browsers (Brave, Firefox, Chrome, etc.)
- HTTP and HTTPs are layer 7 protocols (https://www.cisco.com/)

Functions of Layer 7:
- identifying communication partners
- synchronizing communication.
- provides process-to-process communication |
| 6 | Presentation | - Data in the application layer is in “application format”. A format that applications understands.
-  it needs to be translated to a different format to be sent over the network.
- The presentation layer’s job is to translate between application and network fomarts
- One example function of the Presentation Layer is to encrypt data as it is sent and, decryption as it is received so only the intended recipient can read it.
- Also translate between different application layer formats to ensure the data is in the format the receiving host can understand.
- In short the presentation layer translates data to the appropriate formats. |
| 5 | Session | - Controls dialogue (sessions) between hosts.
- It establishes, manages, and terminates connections between the local application ( for example, your web browser) and a remote application (for example, YouTube). |
| 4 | Transport | - segments and reassembles data for communications between end hosts.
- to reword that, it breaks large pieces of data to smaller segments which can be more easily sent over the network and are less likely to cause transmission problems if error occur.
- provides host-to-host communication also know as end-to-end communication.
- this provides process-to-process communication for applications |
| 3 | Network | - provides connectivity between end hosts on different networks (i.e outside of the LAN).
- provides logical addressing in the form of IP Address.
- provides paths selection between source and destination.
- routes operates at Layer 3. |
| 2 | Data-Link | - provides node-to-node connectivity and data transfer (for example, pc to switch, switch to router and router to router.).
- defines how data is formatted for transmission over a physical medium (for example, copper UTP cables)
- detects and possibly corrects errors that  occur on the physical layer.
- uses layer 2 addressing, separate from layer 3 addressing.
- switches operates at layer 2`` |
| 1 | Physical | - defines the physical characters of a medium used to transfer data between devices. For example, voltage levels, maximum transmission distances, physical connectors, cable specifications etc.
- digital bits are converted to electrical signals for wired connections and radio signals for wireless transmission. |

<aside>
💡 Adjacent-Layer Interaction - the interaction between different layers of the OSI model

</aside>

<aside>
💡 Same-Layer Interaction - the interaction between same layers of the OSI model  of different systems/hosts. For example, the application layer of one hosts interact with the application layer another host. This same layer interation between applications layers is what  allows the application layer to peform its function of identifying communication partners and syncronizing communication.

</aside>

> 💡Network engineers don’t usually work with the top 3 layers, that’s the job for an application developer. Applications developers work with the top layers of the OSI model to connect their applications over networks.
> 

### OSI Model Protocol Data Units (PDUs)

| Layer | PDU |
| --- | --- |
| Application Layer, Presentation Layer & Session Layer | Data |
| Transport Layer | Segment |
| Network Layer | Packet |
| Data-Link Layer | Frame |
| Physical Layer | Bits |

The OSI model (Open System Interconnect) is a theoretical stack of 7 layers that can be used as a reference to understand how a network operates. The model was introduced to standardize networks in a way that allowed multi-vendor systems, prior to this you would only be able to have a one vendor network as the devices could not communicate with other vendor devices.

The Layers

Application (layer 7)

This layer is where the application and user communicates. Application specific protocols are used here such as SMTP (simple mail transfer protocol) for sending emails from Outlook.

Presentation (layer 6)

This layer formats the data in a way that the receiving application can understand it. This layer is also able to encrypt and decrypt date if needed.

Session (layer 5)

This layer is responsible for establishing and terminating of connections between devices.

Transport (layer 4)

The transport layer of OSI Model is used for error handling and sequencing to unsure no data is lost. This layer also adds source and destination port numbers

Network (layer 3)

The network layer handles IP address routing. At this stage of the OSI model the source and destination IP address is added to the data.

Data Link (layer 2)

At this layer the physical address (MAC Address) is added to the data, this includes the source and destination MAC address.

Physical (layer 1)

The physical layer is the lowest layer of the OSI model. Its key responsibility is to carry the data across the physical hardware such as an Ethernet cable to the destination.

## TCP/IP Suite

- A conceptual model and set of communications protocols used in the internet and other networks.
- know as TCP/IP because those are the fundamental protocols in the suite.
- Developed by the United States Department of Defense through DARPA (Defense Advanced Research Project Associaton.)
- it has similar structure to the OSI model but fewer layers.
- This is the model actually in use in  modern networks not the OSI model.
- Note: the OSI model still influences how network engineers  think and talk  about networks.

**TCP/IP Suite Layers**

| Number | Layer Name |
| --- | --- |
| 4 | Application |
| 3 | Transport |
| 2 | Internet |
| 1 | Link |
- The application layer, presentation layer and session layer are essentially equivalent to the application layer for the tcp/ip model.
- The OSI model and TCP/Model both share the transport layer.
- The network layer of the OSI model maps to the internet layer of the TCP/IP model.
- Finally, the data link and physical layer of the OSI model are equivalent to the Link Layer of the TCP/IP model.

## 👨🏽‍🏫QUIZ

1. HTTP data sent from a YOUTUBE webserver is displayed via you web browser. This is an example of wha?
    1. Adjacent-Layer Interaction
    2. Same-Layer Interaction
    3. Encapsulation 
    4. De-encapsulation

1. HTTP data has been encapsulated with 3 separate headers and 1 trailer. What is the Appropriate name of this PDU?
    1. A segment
    2. A packet
    3. Data
    4. A frame
    5. A bit

1. Which layers of the OSI model are most relevant to the role of a network engineer?
    1. Transport-Network-Data Link-Physical
    2. Application-Session-Data Link-Presentation
    3. Presentation-Physical-Application-Session
    4. Network only

1. The Link Layer of the TCP/IP model is equivalent to  what layer, or layers , of the OSI Model?
    1. Presentation
    2. Application-Session
    3. Physical-Data Link   
    4. Network-Transport
    
2. Which Layer of the OSI Model provides host-to-host communications?
    1. Transport
    2. Network
    3. Session
    4. Physical