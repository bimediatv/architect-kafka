# architect-kafka [![NPM version](https://img.shields.io/npm/v/architect-kafka.svg)](https://www.npmjs.com/package/architect-kafka)

Expose [kafka-node](https://github.com/SOHU-Co/kafka-node) as architect plugin

### Installation

```sh
npm install --save architect-kafka
```

### Config Format 

```js
{
}
```

### Usage

Boot [Architect](https://github.com/c9/architect) :

```js
var path = require('path');
var architect = require("architect");

var configPath = path.join(__dirname, "config.js");
var config = architect.loadConfig(configPath);

architect.createApp(config, function (err, app) {
    if (err) {
        throw err;
    }
    console.log("app ready");
});
```

Configure Architect with `config.js` :

See [kafka-node documentation](https://github.com/SOHU-Co/kafka-node#client) for Client options  
See [kafka-node documentation](https://github.com/SOHU-Co/kafka-node#consumer) for Consumer options  
See [kafka-node documentation](https://github.com/SOHU-Co/kafka-node#producer) for Producer options  

```js
module.exports = [
	{
		packagePath: 'architect-kafka',
		client: { // Optional, default client
			// See kafka-node documentation for Client options
			connectionString: 'localhost:2181', // Default localhost:2181
		},
		consumers: {
			// Create a HighLevelConsumer using custom client, consuming from topics topicName1 and topicName2
			consumer1: {
				// Use HighLevelConsumer instead of Consumer, default false
				highLevel: true, 
				// See kafka-node documentation for Client options
			    client: {
			        connectionString: 'localhost:2181',
			        clientId: 'architect-kafka-client'
			    },
				// See kafka-node documentation for Consumer options
			    payloads: [
			        {
			            topic: 'topicName1',
			            offset: 0
			        },
			        {
			            topic: 'topicName2',
			            offset: 10
			        }
			    ],
			    options: {
			        groupId: 'kafka-node-group'
			    }
		    },
			// Create Consumer using default client
		    consumer2: {
		        payloads: [
		            {
		                topic: 'topicName1',
		                offset: 0
		            }
		        ],
		        options: {}
		    }
		},
		producers: {
			// Create a HighLevelProducer using custom client
		    producer1: {
		        highLevel: true, // Use HighLevelproducer instead of Producer, default false
				// See kafka-node documentation for Client options
		        client: {
		            connectionString: 'localhost:2181',
		            clientId: 'architect-kafka-client'
		        },
				// See kafka-node documentation for Producer options
		        options: {}
		    },
			// Create a regular Kafka Producer using default client
		    producer2: {}
		}
	}
];
```
