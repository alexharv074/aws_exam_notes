# SNS

- Publish-subscribe (pub-sub) messaging paradigm. Messages are "pushed" to clients. Push to Apple, Google etc devices.
- Can also deliver messages via SMS, SQS, email, email-JSON, or to an HTTP or HTTPS endpoint. Messages can be customised per protocol.
- Redundancy across AZs automatically taken care of.
- Supports grouping recipients or "subscribers" by "topics". Can have multiple delivery endpoint types (iOS, SMS etc) per topic.

## Pricing

$0.5 per 1 million SNS requests.
$0.06 per 100,000 deliveries over HTTP.
$0.75 per 100 over SMS.
$2 per 100,000 deliveries over email.

