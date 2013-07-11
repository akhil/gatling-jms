gatling-jms
===========
A simple JMS Gatling test library. Gatling provides a simple and relatively pretty performance test framework. http://gatling-tool.org/

Example
===========
Short example, assuming FFMQ on localhost, using a reqreply query, to the queue named 'jmstestq'.

```scala
package com.bluedevel.gatling.jms

import net.timewalker.ffmq3.FFMQConstants
import io.gatling.core.Predef._
import com.bluedevel.gatling.jms.Predef._
import scala.concurrent.duration._
import bootstrap._

class TestJmsDsl extends Simulation {

  val jmsConfig = JmsProtocolBuilder.default
    .connectionFactoryName(FFMQConstants.JNDI_CONNECTION_FACTORY_NAME)
    .url("tcp://localhost:10002")
    .credentials("x", "x")
    .contextFactory(FFMQConstants.JNDI_CONTEXT_FACTORY)

  val scn = scenario("JMS DSL test").repeat(1) {
    exec(jms("req reply testing").reqreply
      .queue("jmstestq")
      .textMessage("hello from gatling jms dsl")
      .addProperty("test_header", "test_value"))
  }

  setUp(scn.protocolConfig(jmsConfig).inject(
       ramp(20 users) over (3 seconds),
       constantRate(30 usersPerSec) during (120 seconds))
    )
}

```

Building
===========
You'll need to download some JARs from here: http://repository.excilys.com/content/groups/public/
```
$ gradle jar
$ cp ~/code/gatling-jms/build/libs/gatling-jms.jar ~/gatling/libs
```

TODO
===========
Plenty. See https://github.com/jasonk000/gatling-jms/issues

License
===========
Copyright jasonk000.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

