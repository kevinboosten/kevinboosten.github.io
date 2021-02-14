---
layout: post
title: 'How I use an OpenAPI spec in my Angular projects'
author: kevin
categories: ['api', 'angular']
tags: ['api', 'Angular']
image: assets/images/posts/2021-02-11/2021-02-11.png
description: 'If you are working on a project that has an OpenAPI spec, then you can generate your Angular Code and even generate a stubbed server for development and test purposes. This reduces time and complexity of integrating with a OpenAPI gateway tremendously.'
featured: false
comments: true
toc: true
---

If you are working on a project that has an OpenAPI spec, then you can generate your Angular Code and even generate a stubbed server for development and test purposes. This reduces time and complexity of integrating with a OpenAPI gateway tremendously.
Let me show you how I use OpenAPI to boost up my productivity!

Here is a brief introduction if you're not familiar with [OpenAPI](https://swagger.io/specification/) in general:

> The OpenAPI Specification (OAS) defines a standard, language-agnostic interface to RESTful APIs which allows both humans and computers to discover and understand the capabilities of the service without access to source code, documentation, or through network traffic inspection. When properly defined, a consumer can understand and interact with the remote service with a minimal amount of implementation logic.

Or maybe you know [Swagger](https://swagger.io/), so what's the difference? Check it out here: [Differences between Swagger and OpenAPI](https://swagger.io/blog/api-strategy/difference-between-swagger-and-openapi/).

## OpenAPI Generator

So your backend colleague gave you the endpoint of their API so you can start integrating your web application with some real data. So what will be your next step? You're quite a Typescript enthousiast and want to make sure that your web application has some type safety, so you start typing out some TS interfaces that we can use. Okay, check ✅.
Next step? Maybe add some abstraction and reusability to your stack? So you create an Angular Service that uses the [HttpClient](https://angular.io/api/common/http/HttpClient) and so wraps the actual endpoint.
Sounds good and eventually this will be a good approach. But it feels a bit repetitive to do this for every project again. Besides that, I think you can spend your time better on building actual features for your application, right?

So what if we could automate these steps to safe some precious time 🧐? In a few steps we can generate Angular specific code based on our OpenAPI spec. Let's get started 👨‍💻!

### Create Angular app

First install the Angular CLI if you dont have this installed already:

```bash
  npm install -g @angular/cli
```

Start with a new angular app and choose the default options:

```bash
  ng new angular-openapi-demo
  cd angular-openapi-demo
```

Start the application to verify everything went well:

```bash
  ng serve
```

### Create an OpenAPI yaml file

A well defined API comes with some documentation. An api built with OpenAPI comes with a yaml, or JSON, spec that describes the actual api. We can build this spec by creating a yaml file in our application.
In order to have a real working api, we will use the well known [JSON Placeholder](https://jsonplaceholder.typicode.com/) public test api.

Add a file `openapi.yaml` to the root of your application and add the following content:

> You can also download the file from the [demo repo](https://github.com/Boosten/angular-openapi-demo/blob/master/openapi.yaml)

```yml
openapi: 3.0.0
info:
  title: JSON Placeholder OpenAPI
  description: Example spec of the well known JSON Placeholder website
  version: 0.1.9
servers:
  - url: https://jsonplaceholder.typicode.com
paths:
  /posts:
    get:
      summary: Returns a list of Posts.
      description: Optional extended description in CommonMark or HTML.
      operationId: GetPosts
      responses:
        '200':
          description: A JSON array of Posts
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Post'
components:
  schemas:
    Post:
      required:
        - id
        - userId
        - title
        - body
      type: object
      properties:
        id:
          type: number
          description: record id
          example: 1
        userId:
          type: string
          description: unique user identifier
          example: 2
        title:
          type: string
          description: title of this Post
          example: sunt aut facere repellat provident occaecati excepturi optio reprehenderit
        body:
          type: string
          description: description of this post
          example: quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto
```

I think that this kind of documenting is quite self explanatory, but let's discuss a couple of things:

- `Post`: this is a way of defining a model. We can use this model by using the [$ref](https://swagger.io/docs/specification/using-ref/) keyword.
- `servers`: here we define one or more base url's for our spec.
- `example`: give a hint about what value to expect. Later on I will explain you why this is handy when creating a simulator.

> Tip! Make working with OpenApi spec easier in VSCode by installing the [Swagger Viewer extension](https://marketplace.visualstudio.com/items?itemName=Arjun.swagger-viewer)

### Generate Angular Services

We're going to generate our Angular code with the CLI of [openapi-generator](https://github.com/OpenAPITools/openapi-generator-cli). We can install this via NPM as devDependency:

```bash
  npm i @openapitools/openapi-generator-cli -D
```

This packages has a lot of [generators available](https://openapi-generator.tech/docs/generators), we're going to use the [typescript-angular](https://openapi-generator.tech/docs/generators/typescript-angular) one.

Add a npm script to your `package.json` file for more convenient usage:

```json
{
  "scripts": {
    // other scripts
    "generate:api": "openapi-generator-cli generate -i ./openapi.yaml -g typescript-angular -o src/app/core/api/v1"
  }
}
```

We use the default configuration ('additional-properties') over here. But you can customize this based on your needs.

One example could be the option `removeOperationIdPrefix` to prevent redundant method names.
Take for example the following operationId in your spec:

```yaml
operationId: Posts_GetPosts
```

The generator will use the operationId to determine the Angular Service name and the method name. If we use the default configuration, our class will look like this:

```ts
// this looks redundant
export class PostsService {
  public postsGetPosts() {}
}

// and when you invoke it, it is redundant and looks weird...
const postsService = new PostsService();
postsService.postsGetPosts();
```

Using the arguments `-p=removeOperationIdPrefix=true` will remove the `Posts_` part of the operationId: `Posts_GetPosts`

```ts
// this looks redundant
export class PostsService {
  public getPosts() {}
}
```

Next step is to actually generate our code with our custom NPM script:

```bash
  npm run generate:api
```

We now have the following directory structure because we told the generator to output (`-o`) to the `src/app/core/api/v1` directory:

![img](/assets/images/posts/2021-02-11/2.png)

### Use generated NgModule and services

The most important parts of the generated code are the following files:

- `posts.services.ts`: the actual Angular service.
- `post.ts`: a TS interface that matches the `Post` model of our OpenAPI spec.
- `api.module.ts`: a NgModule that can be imported to your AppModule.
- `README.md`: [README](https://github.com/Boosten/angular-openapi-demo/blob/master/src/app/core/api/v1/README.md) file with usage instructions.

Add this `ApiModule` to your `AppModule`. This will use the 'default' server endpoint that is available in your openapi.yaml. You can see that in the generated `posts.service.ts`:

```ts
@Injectable({
  providedIn: 'root',
})
export class PostsService {
  protected basePath = 'https://jsonplaceholder.typicode.com';
  // ...
}
```

```ts
// without configuring providers
import { ApiModule } from '';
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [
    ApiModule,
    // make sure to import the HttpClientModule in the AppModule only,
    // see https://github.com/angular/angular/issues/20575
    HttpClientModule,
  ],
  declarations: [AppComponent],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

If you want to override or provide a different endpoint, you can to this by passing in a factory function in the [forRoot](https://angular.io/guide/singleton-services#forRoot) method of the `ApiModule`:

```ts
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';
import {
  ApiModule,
  Configuration,
  ConfigurationParameters,
} from './core/api/v1';

export function apiConfigFactory(): Configuration {
  const params: ConfigurationParameters = {
    basePath: 'https://staging.jsonplaceholder.typicode.com',
  };
  return new Configuration(params);
}

@NgModule({
  imports: [ApiModule.forRoot(apiConfigFactory)],
  declarations: [AppComponent],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

And we can go one step further by moving the basePath to the Angular environment files

```ts
// environment.ts
export const environment = {
  production: false,
  basePath: 'https://dev.jsonplaceholder.typicode.com',
};
```

```ts
// environment.prod.ts
export const environment = {
  production: true,
  basePath: 'https://jsonplaceholder.typicode.com',
};
```

So now we can import the `environment.basePath` variable to configure our ApiModule.

> There are even more variables in the Configuration object that can be set, see [Configuration.ts](https://github.com/Boosten/angular-openapi-demo/blob/master/src/app/core/api/v1/configuration.ts#L3) on the demo repo.

```ts
import { NgModule } from '@angular/core';
import { environment } from '../environments/environment';
import { AppComponent } from './app.component';
import {
  ApiModule,
  Configuration,
  ConfigurationParameters,
} from './core/api/v1';

export function apiConfigFactory(): Configuration {
  const params: ConfigurationParameters = {
    basePath: environment.basePath,
  };
  return new Configuration(params);
}

@NgModule({
  imports: [ApiModule.forRoot(apiConfigFactory)],
  declarations: [AppComponent],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

### Usage example

We can now start using our generated services in our application! Change your `app.component.ts` to this:

```ts
import { Component } from '@angular/core';
import { PostsService } from './core/api/v1';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  // Create `cold` observable - we will be subscribing to this observable in the template
  posts$ = this.postService.getPosts();

  // inject the generated Angular service
  constructor(private postService: PostsService) {}
}
```

And in your `app.component.html` we can use the `posts$` variable by subscribing to it with the [async](https://angular.io/api/common/AsyncPipe) pipe:

```html
<ul>
  <li *ngFor="let post of posts$ | async as list">
    <h2>{{ post.id }} - {{ post.title }}</h2>
    <p>{{ post.body }}</p>
  </li>
</ul>
```

Your browser should now show you a list of Posts from JsonPlaceholder:

![Browser example](/assets/images/posts/2021-02-11/3.png)

🎉 Done! That was all!

### Next steps

In this example I'm generating and putting my code in my project's repository. That's fine for most of my projects because we're using monorepo's and also using client specific API's.
Another approach would be to publish your generated code as a NPM package that can be installed by others. These steps are also described by the OpenAPI generator itself in the [README](https://github.com/Boosten/angular-openapi-demo/blob/master/src/app/core/api/v1/README.md). So it depends on your needs which approach fits better.

## Simulator

<https://github.com/anttiviljami/openapi-backend>

- Explain how to use the api as a simulator

- notice the example data in the spec, this is key for getting a out of the box simulator
  <!-- OpenAPI Example Data in your spec -->
  <!-- https://swagger.io/docs/specification/adding-examples/ -->

  - how to use custom handlers

  - how to add own endpoint (e.g. auth endpoints) to simulate behavior that's outside the spec

  - how can we benefit the simulator in our Angular Application
    - dev setup/speedup development
    - Testing with Cypress - call it integration testing (non persistent!)
