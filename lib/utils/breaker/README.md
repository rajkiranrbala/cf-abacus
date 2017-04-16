abacus-breaker
===

Auto-reclosing circuit breaker for async function calls, inspired by the Akka
breaker.

This module provides a simple auto-reclosing circuit breaker (aka a recloser)
to help protect functions that sometimes fail and avoid failure cascades in
a graph of function calls. The breaker API is inspired by the Akka breaker
with a few refinements to make it a bit simpler and more in line with the
usual Node async function call pattern.

### Usage
```js
const request = require('abacus-request');
const breaker = require('abacus-breaker');

// Circuit breaker options, with the followings
// timeout a call after 60 secs,
// 10 calls must occur before stats matter,
// trip circuit when 50% calls are failures or latent,
// sleep 5 secs before trying again a tripped circuit
const eurekaRequest = breaker(request, {
  name: 'eureka',
  threshold: 60000,
  errors: 10,
  timeout: 50,
  reset: 1000
});

const serviceRequest = breaker(request, {
  name: 'service'
});

eureka.get('http://localhost:9990', (err, res) => {});
serviceRequest.post('http://localhost:9991', (err, res) => {});

```
