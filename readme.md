# EventEmitter

## Overview

The `EventEmitter` class manages event listeners and allows emitting events with improved error handling and additional listener options. It provides methods to add, remove, and emit events while supporting features like priority, groups, conditions, and logging. This documentation provides a comprehensive overview of the EventEmitter class, including its methods, usage examples, and error handling capabilities. If you have any further questions or need additional information, feel free to ask!

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

### Advanced

```typescript
const eventEmitter = new EventEmitter();

/**
 * Listener function to handle button click events.
 * 
 * @param {string} message - Message to be logged.
 */
const onButtonClick = async (message: string): Promise<void> => {
    console.log(message);
    // Simulate async operation
    await new Promise(resolve => setTimeout(resolve, 1000));
    console.log('Async operation completed');
};

/**
 * Another listener function to handle button click events.
 * 
 * @param {string} message - Message to be logged.
 */
const onAnotherButtonClick = async (message: string): Promise<void> => {
    console.log('Another listener:', message);
};

// Add a high priority listener for the 'click' event
eventEmitter.on('click', onButtonClick, { priority: 2, group: 'interaction', condition: (msg) => msg !== '' });
// Add another listener for the 'click' event that will be removed after the first call
eventEmitter.on('click', onAnotherButtonClick, { priority: 1, once: true, group: 'exampleGroup' });

// Emit the 'click' event
await eventEmitter.emit('click', 'Button clicked!');

// Remove all listeners in the 'exampleGroup' group for the 'click' event
eventEmitter.offGroup('click', 'exampleGroup');

// List all events and their listeners
console.log(eventEmitter.listEvents());

// List all groups and their events
console.log(eventEmitter.listGroups());

// Count listeners for 'click' event
console.log(`Number of listeners for 'click' event: ${eventEmitter.countListeners('click')}`);

// Get details of a specific listener
console.log(eventEmitter.getListenerDetails('click', onButtonClick));

// Retrieve the event log for 'click' event
console.log(eventEmitter.getEventLog('click'));

// Clear the event log for 'click' event
eventEmitter.clearEventLog('click');

// Handle errors
eventEmitter.onError((error) => console.error('Error:', error));
```

## License
**EventEmitter** is published as open source. **[MIT License](license.md)** license is used, which
is one of the known open source coding licenses related to license terms. You can get detailed information
about the license terms by visiting the link given below.

- **[MIT License](license.md)**

## Authors
- You can also view the list of contributors to this project by clicking **[this link](https://github.com/srylius/event-emitter/graphs/contributors)**.

[![Facebook](https://img.shields.io/static/v1?message=srylius&style=for-the-badge&logo=facebook&labelColor=1367d4&color=1877F2&logoColor=white&label=%20)](https://facebook.com/srylius)
[![Twitter](https://img.shields.io/static/v1?message=srylius&style=for-the-badge&logo=x&labelColor=1886c9&color=1DA1F2&logoColor=white&label=%20)](https://twitter.com/srylius)
[![Instagram](https://img.shields.io/static/v1?message=srylius&style=for-the-badge&logo=instagram&labelColor=ad2491&color=C32AA3&logoColor=white&label=%20)](https://instagram.com/srylius)
[![YouTube](https://img.shields.io/static/v1?message=srylius&style=for-the-badge&logo=youtube&labelColor=b30202&color=FF0000&logoColor=white&label=%20)](https://youtube.com/@srylius)
[![LinkedIn](https://img.shields.io/static/v1?message=srylius&style=for-the-badge&logo=linkedin&labelColor=0856a3&color=0A66C2&logoColor=white&label=%20)](https://linkedin.com/company/srylius)
[![Github](https://img.shields.io/static/v1?message=srylius&style=for-the-badge&logo=github&labelColor=0f0f0f&color=161717&logoColor=white&label=%20)](https://github.com/srylius)
[![Website](https://img.shields.io/static/v1?message=srylius&style=for-the-badge&logo=dribbble&labelColor=cc5c33&color=eb6a3b&logoColor=white&label=%20)](https://srylius.com)
