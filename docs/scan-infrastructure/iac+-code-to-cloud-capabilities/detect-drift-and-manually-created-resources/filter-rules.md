# Filter rules

You can use filter rules to describe resources and ignore resources. You can use both inclusion and exclusion logic.

Filter rules allow you to build a complex include and exclude expression to include and exclude a set of resources in your workflow. This capability is powered by the expression language [JMESPath](https://jmespath.org).

Filters are applied on a normalized `struct` that contains the following fields:

* Type: Type of the resource, for example, `aws_s3_bucket`
* Id: Id of the resource, for example, `my-bucket-name`
* Attr: Every resource attribute. See this [terraform attributes reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket#attributes-reference) for a full list of supported attributes of a bucket.

{% hint style="info" %}
If you want to filter on `Attr`, enable deep mode in order to have access to the details of a resource.
{% endhint %}

​Examples of filter rules follow.

```
# Will include only S3 bucket in the search
$ snyk iac describe --filter="Type=='aws_s3_bucket'"
# OR (beware of escape your shell special chars between double quotes)
$ snyk iac describe --filter=$'Type==\'aws_s3_bucket\''
# Excludes only s3 bucket named 'my-bucket-name'
$ snyk iac describe --filter=$'Type==\'aws_s3_bucket\' && Id!=\'my-bucket-name\''
# Ignore buckets with an ID prefix of 'terraform-'
$ snyk iac describe --filter=$'!(Type==\'aws_s3_bucket\' && starts_with(Id, \'terraform-\'))'
# Ignore buckets with an ID suffix of '-test'
$ snyk iac describe --filter=$'!(Type==\'aws_s3_bucket\' && ends_with(Id, \'-test\'))'
# Ignore GitHub archived repositories
$ snyk iac describe --to="github+tf" --deep --filter='!(Attr.Archived)'
```
