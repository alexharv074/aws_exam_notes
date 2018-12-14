# CloudWatch

## High-resolution metric

If you set an alarm on a high-resolution metric, you can specify a high-resolution alarm with a period of 10 seconds or 30 seconds, or you can set a regular alarm with a period of any multiple of 60 seconds. There is a higher charge for high-resolution alarms.

## Creating an alarm

You need to configure:

- Period
- Evaluation period - the number of recent data points to evaluate when determining state.
- Datapoints to alarm - the number of data points within the evaluation period that causes a breach.
