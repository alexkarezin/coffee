[![Project Map](https://sourcespy.com/shield.svg)](https://sourcespy.com/github/olgamaciaszekcoffee/)

# Coffee Demo App

Both apps are producers and consumers at the same time, but for the purposes of the demo, producer side contract testing is set up for `barista` module and consumer-side contract testing is set up for `waiter` module.

## FLOW

* Trigger new order via HTTP call in Waiter app (`@PostMapping("/order/{name}/{count}"`), eg. `http POST :8081/order/espresso/1`
* `KafkaTemplate` is used to place a new `Order` on `orders` topic
* The Barista app listens on `orders`; when a new order appears, it processes it into a `Serving` and sends it to `servings` topic; if the beverage name is not matched, a `CoffeeNotAvailableException` is thrown and sent to `errors` topic
* The Waiter app listens on `servings` and `errors` and logs information based on messages received on these topics
