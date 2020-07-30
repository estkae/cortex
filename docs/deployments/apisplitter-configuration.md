# APISplitter configuration

_WARNING: you are on the master branch, please refer to the docs on the branch that matches your `cortex version`_


APISplitter is feature which allows you to split traffic between multiple APIs. This can be useful if you want to roll out updated models.


## APISplitter


```yaml
- name: <string>  # API name (required)
  kind: APISplitter  # must be "SyncAPI", create a synchronous API that holds on to the request and responds only after a prediction has been made
  networking:
    endpoint: <string>  # the endpoint for the API (aws only) (default: <api_name>)
    api_gateway: public | none  # whether to create a public API Gateway endpoint for this API (if not, the load balancer will be accessed directly) (default: public)
  apis:  # list of APIs to use in APISplitter
    - name: <string>  # name of predictor API
      weight: <int>   # proportion of traffic (all APIs add up to 100)
```