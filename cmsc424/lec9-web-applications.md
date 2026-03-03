# Web Applications

## Introduction
- Web/browser apps are the de facto standard user interface
- **REST**: Representation State Transfer, uses standard HTTP requests to execute commands against a server and return data
- For an API to be "RESTful", it must use regular HTTP headers and define endpoints/behavior. It must also use stateless communication and have a uniform interface
- **GraphQL**: An alternative communication protocol where the client decides what data they wish to receive. It also has a single endpoint to send requests with a body specifying desired return fields. GraphQL is good for services with complex, interconnected data models
- Many services offer access through both a REST API and GraphQL endpoint

## Django
- Django is a Python framework for managing HTTP request construction/handling