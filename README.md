# Terraform Module for Creating Hosted Zone and DNS entries

## Introduction
I found myself using the exact same code in my POC repositories for creating a hosted zone and DNS entries for the domains used my the POCs. Following the DRY (**D**on't **R**epeat **Y**ourself) principle, I moved the code to a separate module and updated my repositories to use this module instead.

## Variables
This Terraform module expects the following variables are set:

- ```root_domain``` - This is the root domain that already exists in AWS with a valid ACM certificate

- ```hosted_zone_domain``` - Domain for the hosted zone to create
    
- ```hosted_zone_name``` - Domain name for the hosted zone to create NS record for in the root domain
    
- ```subdomain``` Subdomain to create A record for

- ```a_record_alias_domain_name``` - Domain name of the AWS resource to point the A record to

- ```a_record_alias_zone_id``` - Hosted zone id of the resource to point the A record to

### Example use of variables
The following variables settings:

```tf
root_domain = "yasserabbasi.com
hosted_zone_domain = "playground.yasserabbasi.com"
hosted_zone_name = "playground"
subdomain = "web"
a_record_alias_domain_name = module.alb.alb_dns_name #where  alb_dns_name is the dns name for the alb created in your code
a_record_alias_zone_id = module.alb.alb_zone_id #where alb_zone_id is the zone_id for the alb created in your code
```

will create 
- A hosted zone for "playground.yasserabbasi.com"
- an NS record for "playground" in the root domain "yasserabbasi.com that points to the NS records for the "playground.yasserabbasi.com" hosted zone for DNS delegation.
- An A record in the "playground.yasserabbasi.com" hosted zone for "web" and point it to the alb. I'm using an application load balancer in the above example, but this could be another type of AWS resource e.g. a cloudfront distribution.


License
MIT License

Copyright (c) 2023 Yasser Abbasi

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.