# Temporal Timer Utils

This is a demo library built with [Temporal's TypeScript SDK](https://temporal.io/node) to demonstrate that:

- you can publish libraries that abstract Temporal Workflows and Workflow APIs like Signals, Queries, and Timers
- you can do really cool things with Temporal's `sleep` API.

## Installation

```bash
npm i temporal-timer-utils
```

## Usage

Inside of your Temporal Workflow:

```ts
import { UpdatableTimer, addTimeSignal, timeLeftQuery } from 'temporal-timer-utils'

export async function countdownWorkflow(): Promise<void> {
  const target = Date.now() + 10000;
  const timer = new UpdatableTimer(target);
  console.log('timer set for: ' + new Date(target).toString());
  setListener(addTimeSignal, (deadline) => {
    // send in new deadlines via Signal
    timer.deadline = deadline
    console.log('timer now set for: ' + new Date(deadline).toString());
  }); 
  setListener(timeLeftQuery, () => timer.deadline - Date.now())
  await timer;
  console.log('countdown done!')
}
```

This example `countdownWorkflow` is also exported just for simple demos.
