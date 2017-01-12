# Infrastructure Setup
The whole infrastructure of data package registry is built on AWS services.
## Domain and certificate setup:
AWS has domain hosting service called ```Route53```. This is the only place we are not using aws service.

The extrnal domain: **datapackaged.com**

We mainly have 2 parts:
- ApiGateWay [Custom domain]
- S3
    - Static Files [static.datapackages.com]
    - Data File [bits.datapackaged.com]

### Api Gateway:
For api gateway having custom domain we have to add generated
- Certificate body
- Certificate private key
- Certificate chain
We use ```zerossl``` to generate SSL certificate.
The process is described here:
https://www.pandastrike.com/posts/20160613-ssl-cert-aws-api-gateway-zerossl-letsencrypt

Once we add the certs in ApiGateway it will gevetare cloudfront distribution.
We need to add the ```cloudfront domain``` as CNAME of the external domain provider.

### S3:

- External domain name([bits.staging.datapackagedcom)
- This external domain resolves to cloudfront [*.cloudfront.com]
- Cloudfront resolves to "S3 Bucket (*.s3.amazonaws.com)


In cloud front we can use AWS generated certificates.
> NOTE: One point to mention. ```You can use a certificate stored in AWS Certificate Manager (ACM) in the US East
(N. Virginia) Region, or you can use a certificate stored in IAM.```

#### Building Cloudfront:
- Origin Domain Name= <bucketname>.s3.amazonaws.com
> Note: Bucket name can not have "." in it, e.g. abc.bcd.mybucket

- Cname= {The custom domain name you want to implement}
- Certificate= {Cert generated in AWS Certificate manager in US East(N. Virginia)}
