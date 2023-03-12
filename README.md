# 🐻‍❄️🌲 @augu/pino-transport

> _Noel's opinionated logging transport for [Pino](https://getpino.io)_

**@augu/pino-transport** is an opinionated logging transport for the [Pino](https://getpino.io) logging library. This was made to _not repeat_ what this library entails. If you like it, then install it now:

![default formatter](https://noel-is.gay/images/3d87e6a9.png)
![json formatter](https://noel-is.gay/images/f8d6645d.png)

```shell
$ npm i @augu/pino-transport
$ yarn add @augu/pino-transport
$ pnpm i @augu/pino-transport
```

## Usage

```ts
import pino from 'pino';

const log = pino({
    transports: [
        {
            target: '@augu/pino-transport',
            options: {
                json: true
            }
        }
    ]
});

log.info('Hello, world!');
```

## Custom Transports

You can create custom transports that the transport will transform the logs to. You will need to create a second file that will be serialized to the proper value instead of a plain object; [related issue (pinojs/pino#262)](https://github.com/pinojs/pino-pretty/issues/262)

```ts
// transport.js
import noelPino, { BaseFormatter, type LogRecord } from '@augu/pino-transport';

class MyFormatter extends BaseFormatter {
    override transform(record: LogRecord) {
        return record.msg;
    }
}

export default (options) =>
    noelPino({
        ...options,
        transport: new MyFormatter()
    });

// main.js
import pino from 'pino';

const log = pino({
    transport: {
        target: './transport.js'
    }
});

log.info('Hello, world!');
// => "Hello, world!" is printed instead
```

## Pino Serializers

This library also comes with custom serializers that I recommend setting in the `serializers` option when creating a root Pino logger since it will work better with the Default and Json formatters.

```ts
import { serializers } from '@augu/pino-transport';
import pino from 'pino';

const log = pino({
    serializers: {
        err: serializers.createErrorSerializer(),
        req: serializers.request,
        res: serializers.response
    }
});

log.info({ err: new Error('woof') }, 'waff');
```

## License

**@augu/pino-transport** is released under the **MIT License** with love by [Noel](https://floofy.dev)!
