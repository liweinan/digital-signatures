resteasy-doseta docs

Here are the instructions to deploy the examples to WildFly and test it:

## no-dns

The first step is to start WildFly server(We can use both standalone mode or domain mode for testing, in my example I’ll use standalone mode as default):

```
cute:bin weli$ pwd
/Users/weli/projs/wildfly-8.2.0.Final/bin
cute:bin weli$ ./standalone.sh
...
11:01:11,119 INFO  [org.jboss.as] (Controller Boot Thread) JBAS015874: WildFly 8.2.0.Final "Tweek" started in 5692ms - Started 317 of 378 services (102 services are lazy, passive or on-demand)
```

After WildFly server is started, we need to go to example ‘no-dns’ directory and deploy the example to WildFly server. We can run this command in ‘no-dns’ directory to do the deployment:

```
cute:no-dns weli$ pwd
/Users/weli/projs/digital-signatures/no-dns

cute:no-dns weli$ mvn -q wildfly:deploy
...
INFO: XNIO version 3.3.0.Final
May 02, 2015 11:11:31 AM org.xnio.nio.NioXnio <clinit>
INFO: XNIO NIO Implementation Version 3.3.0.Final
May 02, 2015 11:11:31 AM org.jboss.remoting3.EndpointImpl <clinit>
INFO: JBoss Remoting version 4.0.7.Final
```

If the example is successfully deployed to WildFly server, we can see the relative server log output in WildFly server console:

```
11:11:32,354 INFO  [org.jboss.as.server.deployment] (MSC service thread 1-6) JBAS015876: Starting deployment of "content-signature-example.war" (runtime-name: "content-signature-example.war")
11:11:33,529 INFO  [org.wildfly.extension.undertow] (MSC service thread 1-3) JBAS017534: Registered web context: /content-signature-example
11:11:33,862 INFO  [org.jboss.as.server] (management-handler-thread - 4) JBAS018565: Replaced deployment "content-signature-example.war" with deployment "content-signature-example.war"
```

After the ‘no-dns’ example is successfully deployed, we can run the integration testing provided by the example:

```
cute:no-dns weli$ pwd
/Users/weli/projs/digital-signatures/no-dns

cute:no-dns weli$ mvn -q integration-test
Java HotSpot(TM) 64-Bit Server VM warning: ignoring option MaxPermSize=128m; support was removed in 8.0
```

```
-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running org.jboss.resteasy.tests.signature.SigningTest
d=samplezone.org;s=test;v=1;a=rsa-sha256;c=simple/simple;bh=uU0nuZNNPgilLlLX2n2r+sSE7+N6U4DukIj3rOLvzek=;b=FsU/5XpDoiVkAd5IXBDH1hF5s4F2QHbakF1mNFNNUl5pv3hs9MvBUxqdx6OcaTJoIuQIR0kiKsjcRIOs/PyHU3zrTNsR1SnHMkCMT5TCJ6smVGxki7ASDGMi4iH4kDSQCz68dFoOfdjmOVxBKaUCNvtD0bC1WUcb4k2qUahpD08=
hello world
d=samplezone.org;s=test;v=1;a=rsa-sha256;c=simple/simple;bh=uU0nuZNNPgilLlLX2n2r+sSE7+N6U4DukIj3rOLvzek=;b=l2obFqT7A8J18NIWJPZXmiAJVqYws4qIaDz5NKNZBZy4Ok65bsYhBrAsCi0/nSUKOtA09g0xdx/ZmxLKHikiHlhe2O2C2sM/MV+yoNhtVfw0mb2ICVJxaZgXqd4SqqK7OFbB3JkO5QS1arhnMHX00k0PriEse1R4l4KFF81P010=
We expect this failure: Failed to verify signatures:
 Failed to verify signature.
d=samplezone.org;s=test;x=1430536891;v=1;a=rsa-sha256;c=simple/simple;bh=uU0nuZNNPgilLlLX2n2r+sSE7+N6U4DukIj3rOLvzek=;b=NTBt6k4k9XVoDJkVywTq7MCOYS+GK/W0ZrVrh1dA41seY2S8H2jVzXyJRr+NGjdvqLpXVsOLT2AW3xy8qpvBM55KIRc1KDU3CuWx5jUPf5NLN9eSw8OQzM4AY1ItgApSq9FOAJWIcpI+8UlaFmxg9/lLdS153It6QzacWKxA10E=
hello world
d=samplezone.org;s=test;x=1430536832;v=1;a=rsa-sha256;c=simple/simple;bh=uU0nuZNNPgilLlLX2n2r+sSE7+N6U4DukIj3rOLvzek=;b=UUu+IEgW23X7u9grpffZDI5UGf9UsXw9bN1w4O8++36KrzbyPU/m5LvNI78vPM2/W6hGB5ihG2EXh3irP/xnfAJ8iZLeruraorOeJogwvqiOBzXy9w9WVcJJK47xkknwg0ubYD2Yi+CVRBpcMW/tqmdpHRh4fWDp6DJKxtuAHJ4=
Verification failed: Failed to verify signatures:
 Signature expired
Tests run: 6, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 3.038 sec - in org.jboss.resteasy.tests.signature.SigningTest

Results :

Tests run: 6, Failures: 0, Errors: 0, Skipped: 0
```

From above testing output from client side, we can see all the tests are passed. Next let’s see how to play with the ‘use-dns’ example.

## use-dns