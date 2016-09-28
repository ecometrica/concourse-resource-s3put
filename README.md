# Put only S3 Resource for [Concourse CI](http://concourse.ci)

Extremely simple, yet flexible, s3 put for concourse. Main usage is to
upload for example test result files to s3. Cannot be used as a proper
resource.

## Example pipeline configuration

```yaml
resource_types:
- name: s3-put
  type: docker-image
  source:
    repository: ecometrica/concourse-resource-s3put

resources:
- name: my-s3-put
  type: s3-put
  source:
    access_key_id: {{aws-access-key}}
    secret_access_key: {{aws-secret-key}}
jobs:
- name: A job
  plan:
  - get: ..
  - task: ... 
  - put: my-s3-put
    params:
      s3uri: s3://...
      options:
        - ..
        - ..    
```

## Step parameters.
The flexibility of this resource comes from the fact that the s3uri is
supplied when using put. This means you can sync paths that are based on
build parameters like `$BUILD_PIPELINE_NAME`, etc. The options parameter
is the same as the options parameter to `aws s3 sync`. This means you
can easily change the behaviour of the sync step as needed.