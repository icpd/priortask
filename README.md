# Priority Task
## Introduction
This Go-based project provides a system for managing and executing tasks based on their priority. Specifically, it concurrently executes a collection of tasks and returns the result of the one with the highest priority.
## How It Works
In the context of the Priority Task Processor, a "Task" is defined as any operation with a defined Priority and an Execute function. Here's what these mean:
- **Priority**: An integer indicating relative importance. Higher numbers indicate higher priority.  
- **PerformTask**: The function that performs the given task.    

Task processing involves two major steps:  
1. **Adding Tasks**: Tasks to be executed are added into the Priority Task Processor using the AddTask method. Each task is an instance of a struct that implements the Task interface.
2. **Processing Tasks**: The ProcessTasks function executes all tasks concurrently and retrieves the result from the task with the highest priority. If multiple tasks share the same highest priority, it will return the first result.  
## How to Use
Here's a basic example of how to use the Priority Task Processor:  
1. Implement the Task interface in your task struct.
2. Initiate a new instance of NewTaskController via NewTaskController().
3. Add tasks into the Tasker via AddTask.
4. Call the ProcessTasks method to begin executing tasks. This will return the result of the highest priority task that is valid. If no tasks are valid, it will return an error.
```go
package main

import (
	"context"
	"fmt"
	"time"

	"github.com/icpd/priortask"
)

type MyTask struct {
	p int
	v string
}

func (t MyTask) Priority() int {
	return t.p
}

func (t MyTask) PerformTask() (any, error) {
	time.Sleep(time.Second * time.Duration(t.p))
	return t.v, nil
}

func main() {
	tc := priortask.NewTaskController()

	// add tasks
	tc.AddTask(&MyTask{p: 1, v: "task 1"})
	tc.AddTask(&MyTask{p: 2, v: "task 2"})
	tc.AddTask(&MyTask{p: 3, v: "task 3"})

	// process tasks
	res, err := tc.ProcessTasks(context.Background())
	if err != nil {
		fmt.Println("Error:", err)
		return
	}

	fmt.Println("Result:", res) // Result: task 3
}

```
