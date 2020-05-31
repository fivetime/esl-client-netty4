
Java esl client for netty4
==============================================================================

[![Travis](https://img.shields.io/travis/mgodave/esl-client.svg)](https://travis-ci.org/fivetime/esl-client-netty4)
[![Maven Central](https://img.shields.io/maven-central/v/org.freeswitch.esl.client/org.freeswitch.esl.client.svg)](http://search.maven.org/#artifactdetails%7Corg.freeswitch.esl.client%7Cesl-client-netty4%7C0.9.2%7Cbundle)

**esl-client-netty4** is a Java-based Event Socket Library for the
[FreeSWITCH](https://freeswitch.org/) project.

This project is a fork of the unmaintained, original project at:  
<https://freeswitch.org/stash/projects/FS/repos/freeswitch-contrib/browse/dvarnes/java/esl-client>

Status: In Progress...

About
------------------------------------------------------------------------------
This page documents the org.freeswitch.esl.client library maintained in the freeswitch-contrib git repository. See Java ESL page for overview of other options for using Java with the FreeSWITCH Event Socket.

Features
------------------------------------------------------------------------------
- Licensed under Apache License version 2 http://www.apache.org/licenses/LICENSE-2.0
- Runtime dependency on Netty (Apache License) and SLF4j (MIT License) libraries
- No native library runtime dependencies
- ESL Inbound and Outbound support, see Event Socket and Event Socket Outbound.
- OSGi ready
- Built using maven

Runtime dependencies
------------------------------------------------------------------------------
If you download and install the jar(s) manually, you must also supply the following jars on your Java classpath

- netty-3.10.6.Final.jar for async socket comms
- slf4j-api-1.7.30.jar for logging
- An slf4j implementation, use slf4j-nop.jar if no logging required.

Example
------------------------------------------------------------------------------

```java
package com.ecovate.freeswitch.lb;

import com.google.common.base.Throwables;
import org.freeswitch.esl.client.inbound.Client;
import org.freeswitch.esl.client.inbound.IEslEventListener;
import org.freeswitch.esl.client.internal.IModEslApi.EventFormat;
import org.freeswitch.esl.client.outbound.Context;
import org.freeswitch.esl.client.outbound.IClientHandler;
import org.freeswitch.esl.client.outbound.IClientHandlerFactory;
import org.freeswitch.esl.client.outbound.SocketClient;
import org.freeswitch.esl.client.transport.event.EslEvent;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.net.InetSocketAddress;

public class FreeSwitchEventListener {

  private static Logger logger = LoggerFactory.getLogger(FreeSwitchEventListener.class);

  public static void main(String[] args) {
    try {

      final Client inboudClient = new Client();
      inboudClient.connect(new InetSocketAddress("localhost", 8021), "ClueCon", 10);
      inboudClient.addEventListener(new IEslEventListener() {
        @Override
        public void onEslEvent(EslEvent eslEvent) {

        }
      });
      inboudClient.setEventSubscriptions(EventFormat.PLAIN, "all");

      final SocketClient outboundServer = new SocketClient(
        new InetSocketAddress("localhost", 8084),
        new IClientHandlerFactory() {
          @Override
          public IClientHandler createClientHandler() {
            return new IClientHandler() {
              @Override
              public void handleEslEvent(Context context, EslEvent eslEvent) {
              }

              @Override
              public void onConnect(Context context, EslEvent eslEvent) {
              }
            };
          }
        });


    } catch (Throwable t) {
      Throwables.propagate(t);
    }
  }

}
```

Authors
------------------------------------------------------------------------------

- [Dan Cunningham](mailto:dan.cunningham@readytalk.com)
- [Dave Rusek](mailto:dave.rusek@readytalk.com)
- [David Varnes](mailto:david.varnes@gmail.com) (original author)
- [Tobias Bieniek](https://github.com/Turbo87)

License
------------------------------------------------------------------------------

**esl-client** is licensed under the [Apache License, version 2](LICENSE).

