## AWS General WebApps Workshop

This workshop is a hands-on tutorial of building website on AWS. In lab 1, we will build single-instance WordPress website using [**Amazon EC2**](https://aws.amazon.com/ec2)and [**Amazon RDS**](https://aws.amazon.com/rds/), then we will further improve the architecture of by adding [**Auto Scaling Group**](https://docs.aws.amazon.com/autoscaling/ec2/userguide/AutoScalingGroup.html) , Load Balancer ([**Elastic Load Balancing**](https://aws.amazon.com/elasticloadbalancing/?nc=sn&loc=0)) and CDN ([**AWS CloudFront**](https://aws.amazon.com/cloudfront)) in lab 2 to make the website more scalable, secure and reliable.

![](/images/lab2-architecture.jpg)


## Building the Workshop site

The content of the workshops is built using [hugo](https://gohugo.io/).

### Local Build
To build the content
 * clone this repository
 * [install hugo](https://gohugo.io/getting-started/installing/). The website is currently running on Hugo 0.53, since we have some markdown issues with the latest versions. You can download the exact version here: https://github.com/gohugoio/hugo/releases/download/v0.53/hugo_0.53_Linux-64bit.tar.gz
 * The project uses [hugo learn](https://github.com/matcornic/hugo-theme-learn/) template as a git submodule. To update the content, execute the following code
```bash
pushd themes/learn
git submodule init
git submodule update --checkout --recursive
popd
```
 * Run hugo to generate the site, and point your browser to http://localhost:1313
```bash
hugo serve -D
```

### Containerized Development

The image can also serve as a development environment using [docker-compose](https://docs.docker.com/compose/).
The following command will spin up a container exposing the website at [localhost:1313](http://localhost:1313) and mount `config.toml` and the directories `./content`, `./layouts` and `./static`, so that local changes will automatically be picked up by the development container.

```
$ docker-compose up -d  ## To see the logs just drop '-d'
Starting ec2-spot-workshops_hugo_1 ... done
```

## License

This library is licensed under the Amazon Software License.
