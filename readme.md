# EventEmitter

## Overview

The `EventEmitter` class manages event listeners and allows emitting events with improved error handling and additional listener options. It provides methods to add, remove, and emit events while supporting features like priority, groups, conditions, and logging.

## Installation

npm install selcukcukur/eventemitter

## Usage

First, import the `EventEmitter` class into your project :

```typescript
import { EventEmitter, EventEmitterError, EventListener, ListenerOptions, IEventEmitter } from 'your-event-emitter-package';
```

## API

### Constructor

To create an instance of the EventEmitter class, simply call the constructor :

```typescript
const eventEmitter = new EventEmitter();
```

### Methods

`on(event: string, listener: EventListener, options?: ListenerOptions): void`

Adds an event listener for a specific event.

#### Parameters :
- `event` (string): The name of the event to listen for.
- `listener` (EventListener): The function to be executed when the event is emitted.
- `options` (ListenerOptions, optional): Optional settings for the listener such as priority, once, group, and condition.

#### Example :

```typescript
eventEmitter.on('click', (message) => console.log(message), { priority: 1, once: true, group: 'exampleGroup', condition: (msg) => msg !== '' });
```

