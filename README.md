INVITE Message Generator for the SIP protocol
=============================================

Description
-----------
The SIP INVITE message generator is a C-based program to create SIP INVITE messages from the command line in a flexible way. By using this program we will be able to fake SIP INVITE messages and add the headers, contents and more specific information easily from the command line. This tool will be a fake user agent and will act as a caller within a SIP call.

### INVITE Message Generator allows the user:
+ To select the transport method of the INVITE message.
+ To set the URI of your INVITE message.
+ To add additional headers to your INVITE message.
+ To add additional parts within the INVITE message body as MIME multipart and define its boundary.
+ To add additional headers to the multipart parts of the parts.
+ To include file as the content of the multipart part. (The file could be plain text or binary format).

### Transport used: UDP for SIP is not always recommended
The user must to know that if the sending INVITE is bigger than the MTU, this message will be frag- mented. In the fragmentation process the transport method used is a key factor. UDP transport sim- ply does not provide neither reliability, nor ordering, nor data integrity. TCP detects these problems. If you are using UDP as transport, the message will be sent but none can ensure that the message will be successfully transported to its destiny. In this case, you might want to use TCP as transport method. This is a good example in which use TCP as transport mode in SIP is recommended.

### Adding additional and optional headers to the main SIP INVITE skeleton.

#### Adding a new multipart part
*SDP is always included By default, the INVITE Message Generator has a basic INVITE structure. It contains the basic headers and a valid SDP message as body part. If any multipart part is added, this SDP message will be encapsulated as a multipart part. 
*Adding the multipart content

#### Call-ID generation
The Call-ID header is auto-generated by the INVITE Message Generator. It is a 16 random characters string plus the host address where ther IMG is running.

#### Parameters
![table](https://raw.github.com/luismartingil/img/master/doc/params.png)

#### Additional Multipart Headers
INVITE message generator lets the user to add additional headers to the multipart body. 
The interface could be: 
+ `empty`, if none multipart optional headers need to be added to the multipart.
+ `head1*val1?...headn*valn?`, as the list of optional.

# Dependencies
+ pjsip with ~~bug~~ patch https://trac.pjsip.org/repos/ticket/1387 applied.

# Installation
    # Compiling pjlib
    cd pjproject-1.10
    ./configure --disable-ssl
    make dep
    make
    make install
    # Compiling img
    cd img/src/
    make clean
    make all

### Examples

    ./img -t tcp -u sip:lmartin@10.2.22.55:5060 -b unique-boundary-1 -h MyHeader MyContent -m application pidf+xml "MyHeaderInside*MyContentInside?" text bodies/pidflos/pidflo.xml

    ./img -t tcp -u sip:lmartin@10.22.22.55:5060

    ./img -t tcp -u sip:lmartin@10.22.22.55:5060 -h Geolocation: <cid:alice123@atlanta.example.com>

    ./img -t tcp -u sip:lmartin@10.22.22.55:5060 -b unique-boundary-1 -m application pidf+xml con- tentid_todo pidflo.txt

    ./img -t tcp -u sip:lmartin@10.22.22.55:5060 -h Geolocation <cid:testcid@examples.com> -h Geolocation-Routing no -b unique-boundary-001 -m application pidf+xml examplecontentIDfirst- PIFLO pidflo.txt -m application pidf+xml empty pidflo.txt

### Sniffing the INVITE
    sudo tshark -V -i eth0 not port 22 and sip -R "sip.Method contains "INVITE""
