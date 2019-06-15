# Photon JS generator

The Photon JS generator can be used in a [Prisma project file]() to generate the Photon database client for Node.js and TypeScript. 

## Target

The `photon-js` generator targets [ES2016](https://exploringjs.com/es2016-es2017/) & [Node.js 8.x +](https://nodejs.org/en/download/releases/).

## Example

To invoke the generator, you need to add a [`generator`]() block to your project file and specify the `photon-js` provider:

```
generator js {
  provider = "photon-js"
  output   = "./generated/photon"
}

// ... the file should also contain connectors and a data model definition
```

Once added, you can invoke the generator using the following command:

```
prisma2 generate
```

It will then store the generated Photon API in the specified `./generated/photon` directory. Learn more about the [generated Photon API]().


<!-- ## Fields

The following table describes all _additional_ fields that can be applied to the `photon-js` generator. You can learn more about the standard fields of a generator [here]().

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `target` | Enum () | TBD | Specifies the ECMAScript version for the generated Photon JS. |
 -->

