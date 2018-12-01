# Project Structure

This is the default Project Structure I like to follow for my Spring Projects. Any inputs/suggestions are always welcome.
Email me: karan underscore khosla at outlook dot com

```
com
  kkhosla
        projectName
                  business
                        domain    - Java Business Logic Custom Entity
                        service   - Business Logic
                  data
                      entity      - DB Tables Entity (Representation)
                      repository  - Repository (DB)
                  web
                    application - MVC Controllers
                    service     - Rest Controllers
                    cache       - Cache
  BootstrapApplication.java
```


## Differentiating between domain, model, and entity

domain - referred to the business area
model - model and domain are used interchangeably but ideally in Java we use the term **domain** and in C# we use the term **model**.
entity - refer to the POJO that is persisted in DB

Refer this link: https://stackoverflow.com/questions/15540147/differentiating-between-domain-model-and-entity-with-respect-to-mvc
