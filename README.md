Realtime Community Signage
==========================

This code lets set up and run a cheap realtime community sign with a off-the-shelf router and two scrolling LED signs.  All the content for the sign is managed by a centralized server, where you can pick transit or calendar information to display on the signs.

This is part of the Lost In Boston Realtime project of the [MIT Center for Civic Media](http://civic.mit.edu).  This project is about helping people work together to make their neighborhoods more visitor-friendly. Community groups are partnering with local businesses and institutions to install digital signage that call out useful information in their area. 

Read the INSTALL.md for detailed setup instructions.

Quick Start
-----------

First make your own config file by copying `config.ini.template` to to `config.ini`.

### Serial Port Configuration

We talk to the LED Signs via USB-Serial adaptors (cheap Prolific brand).  Edit your `config.ini` file to be something like this:

```
[Communication]
serial_path=/dev/ttyUSB0
serial_path_2=/dev/ttyUSB1
serial_baudrate=9600
write_to_serial=1
```

### Server Communications

If you just download and start the code, the sign will display content from the `content.xml` file included.  However, it is intended to fetch content from a server, so it can display realtime information.  If you want to have the sign realtime content from a server, you'll need to install our [Community Sign Server software](https://github.com/c4fcm/Community-Sign-Server) on a server.  Then 

Our software is designed to source community transit and calendar information from a central server.  However, out of the box we have included a `content.xml` file that is used instead of live data from a server.  To talk to a server set the variables in the `Server` section of `config.ini` and delete the `content.xml` file.


BOM (Bill of Materials)
-----------------------

For our pilot test, here is the set of hardware we used.

**Total cost $300**

- $200 for two one-line LED Signs from [SignsDirect](http://www.signsdirect.com/Home/LED-Signs-Programmable/7x80-LED-Indoor-Brightness-Sign-Red)
    - or $340 for [semi-outdoor brightness](http://www.signsdirect.com/Home/LED-Signs-Programmable/Red-Programable-Ultra-Bright-LED-Message-Sign)

- $70 Router (Netgear WNR3500L) from [various sources] <http://www.google.com/products/catalog?q=netgear+wnr3500l&um=1&ie=UTF-8&tbm=shop&cid=9147418170759086567&sa=X&ei=yewETo6rGcytgQfW8dHbDQ&ved=0CDAQ8wIwAg>

- $2.70 USB Hub (3 ports) from [monoprice](http://www.monoprice.com/products/product.asp?c_id=103&cp_id=10307&cs_id=1030702&p_id=6631&seq=1&format=3#specification)

- $5 USB Stick (1GB) from anywhere like [microcenter](http://www.microcenter.com/single_product_results.phtml?product_id=0250132)
 
- $8 Project Box from [Mouser](http://www.mouser.com:80/Search/ProductDetail.aspx?R=1591XXFSBKvirtualkey54600000virtualkey546-1591XXFSBK)
    - made by [hammond](http://www.hammondmfg.com/dwg2XXS.htm)

- $4.51 Dual Power Cord Splitter (15P to dual 15R) from
[monoprice](http://www.monoprice.com/products/product.asp?c_id=102&cp_id=10228&cs_id=1022808&p_id=5308&seq=1&format=1#largeimage)

* $6.50 AC Power to Dual C13 splitter (Hosa YIE-406 Dual IEC Power Cable Y-Cable) from [zzounds](http://www.zzounds.com/item--HOSYIE4)
[google shopping](http://www.google.com/products/catalog?hl=en&client=safari&rls=en&q=1+ft+c13++Y+cable&um=1&ie=UTF-8&cid=862520688626840929&sa=X&ei=3OKuTainA8q9tgfR_t3eAw&ved=0CDUQ8gIwAw#)

* $3 Extension Cord:
   - [monoprice 6ft](http://www.monoprice.com/products/product.asp?c_id=102&cp_id=10228&cs_id=1022802&p_id=5299&seq=1&format=2)
    - [monoprice 10ft ](http://www.monoprice.com/products/product.asp?c_id=102&cp_id=10228&cs_id=1022802&p_id=5300&seq=1&format=2)
    - [monoprice 15ft](http://www.monoprice.com/products/product.asp?c_id=102&cp_id=10228&cs_id=1022802&p_id=5301&seq=1&format=2)

* Stand for sign - we laser cut this, using a total of about $3 worth of acrylic

Server API
----------

When the software queries the server it passes along the following arguments on the end of the url:

- *serial*: the serial number set in your `config.ini`
- *secret*: the secret key set in your `config.ini`
- *codeVersion*: the version number of the code
- *protocolVersion*: the verson number of the xml protocol it expects to receive
- *status*: the current status of the sign (one of the `SignController::STATUS_*` constants)

The server's resonse is identical in format to what is shown in the `content.xml` file.  The `<info>` tag holds the content for display on the sign.  For two-line signs, you should separate each line with a EOL.  The only `<command>` recognized for now is `restart`.  This command will restart the client software.