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

**Parameters**
- `event` (string): The name of the event to listen for.
- `listener` (EventListener): The function to be executed when the event is emitted.
- `options` (ListenerOptions, optional): Optional settings for the listener such as priority, once, group, and condition.

**Example**
```typescript
eventEmitter.on('click', (message) => console.log(message), { priority: 1, once: true, group: 'exampleGroup', condition: (msg) => msg !== '' });
```

------
`off(event: string, listener: EventListener): void`

Removes an event listener for a specific event.

**Parameters**
- `event` (string): The name of the event for which the listener should be removed.
- `listener` (EventListener): The function to be removed.

**Example**
```typescript
eventEmitter.off('click', onButtonClick);
```

------
`offGroup(event: string, group: string): void`

Removes all event listeners for a specific event within a specific group.

**Parameters**
- `event` (string): The name of the event.
- `group` (string): The name of the group whose listeners should be removed.

**Example**
```typescript
eventEmitter.offGroup('click', 'exampleGroup');
```

------
`emit(event: string, ...args: any[]): Promise<void>`

Emits an event, triggering all listeners for that event.

**Parameters**
- `event` (string): The name of the event to emit.
- `group` (string): (any[]): Arguments to pass to the event listeners.

**Example**
```typescript
await eventEmitter.emit('click', 'Button clicked!');
```

------
`removeAllListeners(): void`

Removes all listeners from all events.

**Example**
```typescript
eventEmitter.removeAllListeners();
```

------
`listEvents(): Record<string, { listener: EventListener, options?: ListenerOptions, timestamp: Date }[]>`

Lists all events with their listeners.

**Returns**
- `Record<string, { listener: EventListener, options?: ListenerOptions, timestamp: Date }[]>` : A map of events and their listeners.

**Example**
```typescript
console.log(eventEmitter.listEvents());
```

------
`listGroups(): Record<string, Set<string>>`

Lists all groups with their events.

**Returns**
- `Record<string, Set<string>>` : A map of groups and their events.

**Example**
```typescript
console.log(eventEmitter.listGroups());
```

------
`countListeners(event: string): number`

Counts the number of listeners for a specific event.

**Parameters**
- `event` (string): The name of the event to count listeners for.

**Returns**
- `number` : The number of listeners for the event.

**Example**
```typescript
console.log(`Number of listeners for 'click' event: ${eventEmitter.countListeners('click')}`);
```

------
`getListenerDetails(event: string, listener: EventListener): string`

Retrieves details about a specific listener for an event.

**Parameters**
- `event` (string): The name of the event.
- `listener` (EventListener): The listener function to get details for.

**Returns**
- `string` : Details about the listener.

**Example**
```typescript
console.log(eventEmitter.getListenerDetails('click', onButtonClick));
```

------
`getEventLog(event: string): { timestamp: Date; args: any[] }[]`

Retrieves the event log for a specific event.

**Parameters**
- `event` (string): The name of the event.

**Returns**
- `Array<{ timestamp: Date; args: any[] }>` : The log of the event.

**Example**
```typescript
console.log(eventEmitter.getEventLog('click'));
```

------
`clearEventLog(event: string): void`

Clears the event log for a specific event.

**Parameters**
- `event` (string): The name of the event.

**Example**
```typescript
eventEmitter.clearEventLog('click');
```

------
`onError(listener: (error: EventEmitterError) => void): void`

Registers an error listener to handle errors.

**Parameters**
- `listener` ((error: EventEmitterError) => void): The function to handle errors.

**Example**
```typescript
eventEmitter.onError((error) => console.error('Error:', error));
```

------
`onError(listener: (error: EventEmitterError) => void): void`

Registers an error listener to handle errors.

**Parameters**
- `listener` ((error: EventEmitterError) => void): The function to handle errors.

**Example**
```typescript
eventEmitter.onError((error) => console.error('Error:', error));
```

### Errors

`EventEmitterError`

A custom error class for handling event emitter related errors.

**Example**
```typescript
throw new EventEmitterError('An error occurred in the event emitter.');
```

-----
`EventListenerError`

A custom error class for handling listener specific errors in the event emitter.

**Parameters**
- `message` (string): The error message.
- `eventName` (string): The name of the event.
- `listenerName` (string): The name of the listener.

**Example**
```typescript
throw new EventListenerError('An error occurred in the listener.', 'click', 'onButtonClick');
```

-----
`EventGroupError`

A custom error class for handling group specific errors in the event emitter.

**Parameters**
- `message` (string): The error message.
- `groupName` (string): The name of the group.

**Example**
```typescript
throw new EventGroupError('An error occurred in the group.', 'exampleGroup');
```

### Example Usage


